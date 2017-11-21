# ElasticSearch-5.6.3-with-IK
使用IKAnalysis作为中文分词器的ES，可直接部署于服务器，节省一下午的部署时间。。

## 使用方法
* 直接下载zip包，DOWNLOAD ZIP；
* 上传到需部署ElasticSearch的服务器目录；
* 在目录下解压：unzip elasticsearch-5.6.3-with-ik.zip；
* 按需修改ES配置config/elasticsearch.yml，本配置包含了两个节点的ES集群；
* 启动ES不能用root用户，方法参见https://my.oschina.net/topeagle/blog/591451?fromerr=mzOr2qzZ；


## 相关版本和使用说明
* ElasticSearch为5.6.3版本（名字也看出来了）
* 中文分词器IKAnalyzer，项目详见：https://github.com/medcl/elasticsearch-analysis-ik, 使用了tag/v5.6.3
* 5.x以上的ES，不能在节点的配置中进行索引级别的配置，因此只能针对某个index配置指定分词器，方法见下文

## 让中文分词器生效步骤

**1）把索引关闭：**

curl -XPOST 'localhost:9200/knowledge/_close?pretty'

**2）更新设置：**

curl -XPUT 'http://localhost:9200/knowledge/_settings?preserve_existing=true&pretty' -d '{"index":{"analysis":{"analyzer": {"default": {"type":"ik_smart"}},"tokenizer": {"default": {"type":"ik_max_word"}}}}}'

**3）把索引重新打开**

curl -XPOST 'localhost:9200/knowledge/_open?pretty'

**4）测试**


*默认分词器测试方式*

curl 'http://localhost:9200/knowledge/_analyze?pretty=true' -d '
{
"text":"中华人民共和国"
}'

*指定分词器的测试方法：*

curl 'http://localhost:9200/knowledge/_analyze?tokenizer=ik_max_word&pretty=true' -d '
{
"text":"中华人民共和国"
}'

