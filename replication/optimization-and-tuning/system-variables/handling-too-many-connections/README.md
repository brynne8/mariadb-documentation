# Handling Too Many Connections

Systems that get too busy can return the `too_many_connections` error.

When the number of [threads_connected](/kb/en/server-status-variables/#threads_connected) exceeds the <a undefined>max_connections</a> server variable, it's time to make a change. Viewing the [threads_connected](/kb/en/server-status-variables/#threads_connected) status variable shows only the current number of connections, but it's more useful to see what the value has peaked at, and this is shown by the [max_used_connections](/kb/en/server-status-variables/#max_used_connections) status variable.

This error may be a symptom of slow queries and other bottlenecks, but if the system is running smoothly this can be addressed by increasing the value of max_connections.