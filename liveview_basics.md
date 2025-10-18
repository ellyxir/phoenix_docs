---
title: LiveView Basics
subject: Tutorial
keywords:
  - phoenix
authors:
  - name: Ellyxir
    email: ellyse@ellyxir.com
---

Put file in `lib/<app_name>_web/live/`

```{code-block} elixir
defmodule <AppName>Web.MyModule do
  use <AppName>Web, :live_view

  def render(assigns) do
    ~H"""
    clicks: {@num_clicks}
    <button phx-click="inc_clicks">+</button>
    """
  end

  def mount(_params, _session, socket) do
    clicks = 1
    {:ok, assign(socket, :num_clicks, clicks)}
  end

  def handle_event("inc_clicks", _params, socket) do
    {:noreply, update(socket, :num_clicks, &(&1 + 1))}
  end
end
```

Edit the router:

```{code-block} elixir
:linenos: true
:emphasize-lines: 12
defmodule <AppName>Web.Router do
  use <AppName>Web, :router

  pipeline :browser do
    ...
  end

  scope "/", <AppName>Web do
    pipe_through :browser
    ...

    live "/clicky", MyModule 
  end
end
```

Notice on line 12 we added a `live` HTTP verb. This is how the system knows we will be usine LiveView.

