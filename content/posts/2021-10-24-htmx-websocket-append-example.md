---
title: "An HTMX example appending content from a websocket"
date: 2021-10-24
tags:
- htmx
summary: Updating the page using HTMX and messages from a websocket.
---

I want to present an example of appending content to the page when using HTMX, receiving data from a websocket. I
couldn't find a handy example to use, and so had to investigate myself.

## Example server

We will set up an example server using [FastAPI](https://fastapi.tiangolo.com/) in Python. This server will show a basic HTML template containing a
single text input for new messages, and 

```python
import time

from fastapi import FastAPI, Request, WebSocket
from fastapi.templating import Jinja2Templates

templates = Jinja2Templates(directory="templates")


app = FastAPI()


@app.get("/")
async def get(request: Request):
    return templates.TemplateResponse("index.html", {"request": request})


@app.websocket("/ws")
async def time_handler(websocket: WebSocket):
    await websocket.accept()
    # response content
    content = """
        <div hx-swap-oob="beforeend:#content">
        <p>{time}: {message}</p>
        </div>
    """
    while True:
        msg = await websocket.receive_json()
        await websocket.send_text(
            content.format(time=time.time(), message=msg["chat_message"])
        )
```

where the `index.html` template is as follows:

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- include htmx -->
    <script
      src="https://unpkg.com/htmx.org@1.6.0"
      crossorigin="anonymous"
    ></script>
  </head>

  <body>
    <h1>Hello world</h1>

    <!-- websocket connection -->
    <div hx-ws="connect:/ws">
      <!-- input for new messages, send a message to the backend
      via websocket -->
      <form hx-ws="send:submit">
        <input name="chat_message" />
      </form>
    </div>

    <!-- location for new messages from the server -->
    <div id="content"></div>

  </body>
</html>
```

## Key points

The websocket connection is created with the `hx-ws` attribute. The syntax is: `hx-ws="connect:<endpoint>"`.

```html
<div hx-ws="connect:/ws">
</div>
```

Within this we include the single control that handles input from the user:

```html
<form hx-ws="send:submit">
  <input name="chat_message" />
</form>
```

So far so htmx documentation. Note in the documentation, the example shows _replacing_ an element with a new copy from
the server. This is not the behaviour we want. Instead we want to add a new message to the page without removing the old
ones - like a chat server.

The response from the server looks like this:

```html
<div hx-swap-oob="beforeend:#content">
  <p>{{ time }}: {{ message }}</p>
</div>
```

The important part is `hx-swap-oob="beforeend:#content"`. This means the response should update a different HTML element
than the one specified. In addition, we are using `beforeend:<css-selector>` to select which element to update.
`beforeend` means that the child element is appended to the _last_ child element of the selected element. So the final
HTML after adding a few messages will be:

```html
<div id="content">
  <p>1635036095: message 1</p>
  <p>1635036109: message 2</p>
  <p>1635036117: message 3</p>
</div>
```

## Resources

These links helped me understand this problem:

* [Building a Twitter clone app in htmx](https://dev.to/rajasegar/building-a-twitter-clone-app-in-htmx-42o6)
* [HTMX issue](https://github.com/bigskysoftware/htmx/issues/552)
* [HTMX documentation](https://htmx.org/attributes/hx-ws/)

