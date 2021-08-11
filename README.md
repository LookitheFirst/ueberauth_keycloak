# Überauth Keycloak

> Keycloak OAuth2 strategy for Überauth.

## Acknowledgements

This repository is based on the work of [mtchavez/ueberauth_keycloak](https://github.com/mtchavez/ueberauth_keycloak).

## Installation

1. Add `:ueberauth_keycloak_strategy` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [{:ueberauth_keycloak_strategy, "~> 0.2"}]
    end
    ```

1. Add the strategy to your applications:

    ```elixir
    def application do
      [applications: [:ueberauth_keycloak_strategy]]
    end
    ```

1. Add Keycloak to your Überauth configuration:

    ```elixir
    config :ueberauth, Ueberauth,
      providers: [
        keycloak: {Ueberauth.Strategy.Keycloak, [
          default_scope: "read_user",
          client_id: System.get_env("KEYCLOAK_CLIENT_ID"),
          client_secret: System.get_env("KEYCLOAK_CLIENT_SECRET"),
          redirect_uri: System.get_env("KEYCLOAK_REDIRECT_URI")
        ]}
      ]
    ```

1.  Include the Überauth plug in your controller:

    ```elixir
    defmodule MyApp.AuthController do
      use MyApp.Web, :controller

      pipeline :browser do
        plug Ueberauth
        ...
       end
    end
    ```

1.  Create the request and callback routes if you haven't already:

    ```elixir
    scope "/auth", MyApp do
      pipe_through :browser

      get "/:provider", AuthController, :request
      get "/:provider/callback", AuthController, :callback
    end
    ```

1. You controller needs to implement callbacks to deal with `Ueberauth.Auth` and `Ueberauth.Failure` responses.

For an example implementation see the [Überauth Example][example-app] application
on how to integrate other strategies. Adding Keycloak should be similar to Github.

## Calling

Depending on the configured url you can initial the request through:

    /auth/keycloak

Or with options:

    /auth/keycloak?scope=profile


```elixir
config :ueberauth, Ueberauth,
  providers: [
    keycloak: {
      Ueberauth.Strategy.Keycloak, [
        default_scope: "profile"
      ]
    }
  ]
```

## Documentation

The docs can be found at [ueberauth_keycloak][package-docs] on [Hex Docs][hex-docs].

[example-app]: https://github.com/ueberauth/ueberauth_example
[hex-docs]: https://hexdocs.pm
[package-docs]: https://hexdocs.pm/ueberauth_keycloak_strategy
