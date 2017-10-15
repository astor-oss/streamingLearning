# 1. taskmananger.network.numberOfBuffers设置

### I get an error message saying that not enough buffers are available. How do I fix this? {#i-get-an-error-message-saying-that-not-enough-buffers-are-available-how-do-i-fix-this}

If you run Flink in a massively parallel setting \(100+ parallel threads\), you need to adapt the number of network buffers via the config parameter`taskmanager.network.numberOfBuffers`. As a rule-of-thumb, the number of buffers should be at least`4 * numberOfNodes * numberOfTasksPerNode^2`. See[Configuration Reference](https://ci.apache.org/projects/flink/flink-docs-release-0.8/config.html)for details







