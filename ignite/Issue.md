# IgniteIssue
1. 2个节点同时加入导致Merge异常问题
Answer: https://issues.apache.org/jira/browse/IGNITE-7366
Issue:  https://mail.google.com/mail/u/0/#inbox/160fcd2430d61d5f

2. Ignite IPV4 and IPV6
```
I see that you use both ipv4 and ipv6 stacks, I think it can somehow affect this behavior, so, I would recommend enabling -Djava.net.preferIPv4Stack=true 
```

3. Ignite wal mmap config
```java
IgniteSystemProperties.getBoolean(IGNITE_WAL_MMAP, true);
```
- question
    - the function while set IGNITE_WAL_MMAP


4. Ignite Wal config
```java
DataStorageConfiguration dsCfg = new DataStorageConfiguration().setDefaultDataRegionConfiguration(
        new DataRegionConfiguration().setMaxSize(100 * 1024 * 1024).setPersistenceEnabled(true))
    .setWalMode(WALMode.LOG_ONLY)
    .setWalCompactionEnabled(false)
    .setWalSegmentSize(WAL_SEGMENT_SIZE)
    .setConcurrencyLevel(Runtime.getRuntime().availableProcessors() * 4);
```

5. Ignite throw Exception

```java
Caused by: java.nio.channels.ClosedByInterruptException
:call <SNR>122_SparkupNext()
at java.nio.channels.spi.AbstractInterruptibleChannel.end(AbstractInterruptibleChannel.java:202)

at sun.nio.ch.FileChannelImpl.readInternal(FileChannelImpl.java:746)
at sun.nio.ch.FileChannelImpl.read(FileChannelImpl.java:727)
at org.apache.ignite.internal.processors.cache.persistence.file.RandomAccessFileIO.read(RandomAccessFileIO.java:62)
at org.apache.ignite.internal.processors.cache.persistence.file.FilePageStore.read(FilePageStore.java:322)
... 60 more
```

This should not generally happen under normal load, however if you're seeing such errors, consider setting the following Java argument for Ignite:
-DIGNITE_USE_ASYNC_FILE_IO_FACTORY=true

6. 
