## compress_chunk() <tag type="community">Community</tag>

The compress_chunk function is used to compress a specific chunk. This is
most often used instead of the
[add_compression_policy](/compression/add_compression_policy/) function, when a user
wants more control over the scheduling of compression. For most users, we
suggest using the policy framework instead.

<highlight type="tip">
You can get a list of chunks belonging to a hypertable using the
`show_chunks` [function](/api/latest/hypertable/show_chunks/).
</highlight>

### Required Arguments

|Name|Type|Description|
|---|---|---|
| `chunk_name` | REGCLASS | Name of the chunk to be compressed|


### Optional Arguments

|Name|Type|Description|
|---|---|---|
| `if_not_compressed` | BOOLEAN | Setting to true will skip chunks that are already compressed, but the operation will still succeed. Defaults to false.|

### Returns

|Column|Description|
|---|---|
| `compress_chunk` | (REGCLASS) Name of the chunk that was compressed|


### Sample Usage 
Compress a single chunk.

``` sql
SELECT compress_chunk('_timescaledb_internal._hyper_1_2_chunk');
```
