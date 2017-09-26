# nodejs-express-handlebars-livereload
An express generated handlebars template engine skeleton with live reload.

Whenever a file is changed, the page is reloaded instantly.

No more cycling between restarting node server and refreshing browser again.

You won't need a browser extension. It just works.

Express skeleton is created with [express-generator](https://expressjs.com/en/starter/generator.html):

    express nodejs-express-handlebars-livereload --hbs
# Installation

    git clone https://github.com/ramazanpolat/nodejs-express-handlebars-livereload.git
    cd nodejs-express-handlebars-livereload
    npm install

# Running

    npm run live

# How does it work?

It uses [supervisor](https://github.com/petruisfan/node-supervisor) to restart node on server side and [socket.io](https://github.com/socketio/socket.io) to reload page on client side.

The client part of socket.io code is implemented in /views/layout.hbs as:

    <script>
        var waitingToReconnect = false;
        var socket = io(window.location.host);
        socket.on('connect', function(){
            if (waitingToReconnect)
                location.reload();
        });
        socket.on('disconnect', function(){
            waitingToReconnect = true;
        });
    </script>


So any view rendered using this layout will be reloaded.

# Notes

If there are additional folders you want to be ignored by superviser(maybe files or folders generated by your IDE), go to package.json file and change this line:

    "live": "supervisor -i .idea,cmake-build-debug,.git  bin/www.js"
    
to

    "live": "supervisor -i .idea,cmake-build-debug,.git[,your-directory-to-e-ignored]  bin/www.js"

