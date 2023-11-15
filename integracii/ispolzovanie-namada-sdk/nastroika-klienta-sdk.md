# Настройка клиента SDK

После импорта sdk в проект его можно использовать для взаимодействия с блокчейном Namada. Предположим, что у нас есть узел, работающий по ip и порту `127.0.0.1:26657`, и мы хотим отправить транзакцию в сеть.

SDK может быть использован для различных целей, но в данном примере мы будем использовать его для отправки транзакции в сеть.

Для начала нам необходимо реализовать клиент, чтобы мы могли взаимодействовать с работающим узлом.

```rust
use reqwest::{Client, Response as ClientResponse};
 
 
pub struct SdkClient {
    url: String,
    client: Client,
}
 
impl SdkClient {
    pub fn new(url: String) -> Self {
        Self {
            client: Client::new(),
            url,
        }
    }
 
    pub async fn post(&self, body: String) -> Result<ClientResponse, reqwest::Error> {
        self.client
            .post(format!("http://{}", &self.url))
            .body(body)
            .send()
            .await
    }
}
```

Это позволяет нам использовать `Client` из `reqwest` (внешняя библиотека) для отправки транзакции в сеть.

Нам также необходимо определить некоторые функции, которые клиент будет использовать для взаимодействия с сетью.

```rust
#[async_trait::async_trait]
impl ClientTrait for SdkClient {
    type Error = Error;
 
    async fn request(
        &self,
        path: String,
        data: Option<Vec<u8>>,
        height: Option<BlockHeight>,
        prove: bool,
    ) -> Result<EncodedResponseQuery, Self::Error> {
        let data = data.unwrap_or_default();
        let height = height
            .map(|height| {
                tendermint::block::Height::try_from(height.0)
                    .map_err(|_err| Error::InvalidHeight(height))
            })
            .transpose()?;
        let response = self
            .abci_query(
                Some(std::str::FromStr::from_str(&path).unwrap()),
                data,
                height,
                prove,
            )
            .await?;
 
        match response.code {
            Code::Ok => Ok(EncodedResponseQuery {
                data: response.value,
                info: response.info,
                proof: response.proof,
            }),
            Code::Err(code) => Err(Error::Query(response.info, code)),
        }
    }
 
    async fn perform<R>(&self, request: R) -> Result<R::Response, tm_rpc::Error>
    where
        R: tm_rpc::SimpleRequest,
    {
        let request_body = request.into_json();
        let response = self.post(request_body).await;
 
        match response {
            Ok(response) => {
                let response_json = response.text().await.unwrap();
                R::Response::from_string(response_json)
            }
            Err(e) => {
                let error_msg = e.to_string();
                Err(tm_rpc::Error::server(error_msg))
            }
        }
    }
}
```

Теперь мы готовы использовать этого клиента для отправки транзакций.
