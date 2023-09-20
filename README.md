<div align="center">
  <h1>Orca</h1>
  <p>
    <strong>Orca is a LLM Orchestrator Framework written in Rust. It is designed to be a simple, easy to use, and easy to extend framework for creating LLM Orchestrators. It is currently in development and it's functionality is limited.</strong>
  </p>
  <p>

<!-- prettier-ignore-start -->

[![CI](https://github.com/scrippt-tech/orca/actions/workflows/rust.yml/badge.svg)](https://github.com/scrippt-tech/orca/actions/workflows/rust.yml)

<!-- prettier-ignore-end -->

  </p>
</div>

# Set up
To set up Orca, you will need to install Rust. You can do this by following the instructions [here](https://www.rust-lang.org/tools/install). Once you have Rust installed, you can add Orca to your Cargo.toml file as a dependency:
```toml
[dependencies]
orca = { git = "https://github.com/scrippt-tech/orca" }
```

# Examples
Orca supports simple LLM chains and sequential chains. It also supports reading PDF and HTML records (documents). Following is a simple example on how to use Orca.
```rust
use orca::chains::chain::LLMChain;
use orca::chains::traits::Execute;
use orca::prompts;
use orca::prompt::prompt::PromptTemplate;
use orca::llm::openai::client::OpenAIClient;
use serde::Serialize;

#[derive(Serialize)]
pub struct Data {
    country1: String,
    country2: String,
}

#[tokio::main]
async fn main() {
    let client = OpenAIClient::new();
    let res = LLMChain::new(
        Some("MyChain"),
        &client,
        prompts!(
            ("user", "What is the capital of {{country1}}"),
            ("ai", "Paris"),
            ("user", "What is the capital of {{country2}}")
        ),
    )
    .execute(
        &Data {
            country1: "France".to_string(),
            country2: "Germany".to_string(),
        },
    )
    .await
    .unwrap();
    assert!(res.contains("Berlin") || res.contains("berlin"));
}
```

