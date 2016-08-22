##倒排索引
1.**单词－文档矩阵**  

单词-文档矩阵是表达两者之间所具有的一种包含关系的概念模型，搜索引擎的索引其实就是实现“单词-文档矩阵”的具体数据结构，其中倒排索引是最佳的实现方式。  
    ![](http://o793hh4oz.bkt.clouddn.com/image/jpg/1347267421_6535.jpg)  


2.**基本概念**  

*文档(Document)*  
    文档是以文本形式存在的存储对象，比如一个网页，一条短信等等。

*文档集合(Document Collection)*  
    由若干文档构成的集合称之为文档集合。

*文档编号(Document ID)*  
    在搜索引擎内部，会将文档集合内每个文档赋予一个唯一的内部编号，以此编号来作为这个文档的唯一标识。

*单词编号(Word ID)*  
    与文档编号类似，搜索引擎内部以唯一的编号来表征某个单词，单词编号可以作为某个单词的唯一标识。

*倒排索引(Inverted Index)*  
    倒排索引是实现“单词-文档矩阵”的一种具体存储形式，通过倒排索引，可以根据单词快速获取包含这个单词的文档列表。倒排索引主要由两个部分组成：“单词词典”和“倒排文件”。

*单词词典(Lexicon)*  
    搜索引擎的通常索引单位是单词，单词词典是由文档集合中出现过的所有单词构成的字符串集合，单词词典内每条索引项记载单词本身的一些信息以及指向“倒排列表”的指针。

*倒排列表(PostingList)*  
    倒排列表记载了出现过某个单词的所有文档的文档列表及单词在该文档中出现的位置信息，每条记录称为一个倒排项(Posting)。根据倒排列表，即可获知哪些文档包含某个单词。

*倒排文件(Inverted File)*  
    所有单词的倒排列表往往顺序地存储在磁盘的某个文件里，这个文件即被称之为倒排文件，倒排文件是存储倒排索引的物理文件。

![](http://o793hh4oz.bkt.clouddn.com/image/jpg/1347267600_5088.jpg)

3.**案例**  

该文档集合包含5个文档  
![](http://o793hh4oz.bkt.clouddn.com/image/jpg/1347267642_4728.jpg)  

通过分词系统对每个文档进行分析，生成一个单词序列，并给每个单词一个唯一id，同时记录下哪些文档包含这个单词，这就得到了最简单的倒排索引。在实际的搜索引擎系统中，并不存储倒排索引项中的实际文档编号，而是代之以文档编号差值。文档编号差值是倒排列表中相邻的两个倒排索引项文档编号的差值，一般在索引构建过程中，可以保证倒排列表中后面出现的文档编号大于之前出现的文档编号，所以文档编号差值总是大于0的整数。之所以要对文档编号进行差值计算，主要原因是为了更好地对数据进行压缩，原始文档编号一般都是大数值，通过差值计算，就有效地将大数值转换为了小数值，而这有助于增加数据的压缩率。  
![](http://o793hh4oz.bkt.clouddn.com/image/jpg/1347267660_2611.jpg)  

相对复杂一些的倒排索引，包含了词频信息  
![](http://o793hh4oz.bkt.clouddn.com/image/jpg/1347267682_4417.jpg)

4.**正排索引**  

*what*  
    本质上说，正排索引以文档编号为视角看待索引词，也就是通过文档编号去找索引词。任给一个文档编号，能够知道它包含了哪些索引词、这些索引词分别出现的次数，以及索引词出现的位置。然而全文索引是通过关键词来检索，而不是通过文档编号来检索，因此正排索引不能满足全文检索的要求。

*两者的关系*  
    存在这样两个空间，一个称为"索引词空间"，一个称为"文档空间"。正排索引可以理解成一个定义在文档空间到索引词组空间的一个映射，任意一个文档对应唯一的一组索引词；而倒排索引可以理解成一个定义在索引词空间到文档组空间的一个映射。任意一个索引词对应唯一的一组该索引词其命中的文档。因此从文档到正排索引，进而从正排索引到倒排索引就是理顺这种关系的过程。使得给出一个索引词，就能通过倒排索引能够找到其命中的文档，以及位置信息。


##ElasticSearch
请求：  

    curl -Xpost 'http://10.90.4.44:10201/guessyoulike_2016-06-23/userdata/_search?pretty&from=0&size=1&_source=id,uid,hotelPC,ticketPC,flightPC' -d
    '{"query":{"match":{"id":64852828}}}'   

运行结果：

    {
      "took" : 11,         //请求消耗时间，单位毫秒
      "timed_out" : false, //有没有超时
      "_shards" : {
        "total" : 5,       //查询的分片数量
        "successful" : 5,  //成功返回结果的分片数量
        "failed" : 0       //失败的数量
      },
      "hits" : {            
        "total" : 2,       //符合条件的文档总数
        "max_score" : 12.719964,  //最高得分
        "hits" : [ {       //结果数组
          "_index" : "guessyoulike_2016-06-23",
          "_type" : "userdata",
          "_id" : "AVWGWOs54q6RF88kJDh0",  //自动生成的唯一id
          "_score" : 12.719964,
          "_source" : {         //请求的数据
            "flightPC" : 47,
            "uid" : "4C2E0F49-C579-9034-9381-1D712B687B73",
            "hotelPC" : 2,
            "ticketPC" : 0,
            "id" : 64852828
          }
        } ]
      }
    }

size＝2:

    {
      "took" : 30,
      "timed_out" : false,
      "_shards" : {
        "total" : 5,
        "successful" : 5,
        "failed" : 0
      },
      "hits" : {
        "total" : 2,
        "max_score" : 12.719964,
        "hits" : [ {
          "_index" : "guessyoulike_2016-06-23",
          "_type" : "userdata",
          "_id" : "AVWGWOs54q6RF88kJDh0",
          "_score" : 12.719964,
          "_source" : {
            "flightPC" : 47,
            "uid" : "4C2E0F49-C579-9034-9381-1D712B687B73",
            "hotelPC" : 2,
            "ticketPC" : 0,
            "id" : 64852828
          }
        }, {
          "_index" : "guessyoulike_2016-06-23",
          "_type" : "userdata",
          "_id" : "AVWGWQCW4q6RF88kJEv8",
          "_score" : 12.7168455,
          "_source" : {
            "flightPC" : 47,
            "uid" : "4C2E0F49-C579-9034-9381-1D712B687B73",
            "hotelPC" : 2,
            "ticketPC" : 0,
            "id" : 64852828
          }
        } ]
      }
    }

请求：

    curl -Xpost 'http://10.90.4.44:10201/_search?pretty&from=0&size=2&_source=id,uid,hotelPC,ticketPC,flightPC,trainBook,vacationPC,date' -d '
    {
        "query": {
            "bool": {       //布尔查询
                "must": [   //must_not,should
                    {
                        "match": {
                            "haslbs": 0
                        }
                    },
                    {
                        "match": {
                            "uid": "14BFADB2-4A60-7A50-C892-A75B35DB1A5B"
                        }
                    }
                ]
            }
        }
    }
    '

运行结果：

    {
      "took" : 1255,
      "timed_out" : false,
      "_shards" : {
        "total" : 30,
        "successful" : 30,
        "failed" : 0
      },
      "hits" : {
        "total" : 4,
        "max_score" : 13.029651,
        "hits" : [ {
          "_index" : "guessyoulike_2016-06-23",
          "_type" : "userdata",
          "_id" : "AVWGWJH34q6RF88kI8n9",
          "_score" : 13.029651,
          "_source" : {
            "date" : "2016-06-23 09:48:58.0",
            "flightPC" : 0,
            "uid" : "14BFADB2-4A60-7A50-C892-A75B35DB1A5B",
            "hotelPC" : 0,
            "ticketPC" : 0,
            "vacationPC" : 0,
            "id" : 64839549,
            "trainBook" : 0
          }
        }, {
          "_index" : "guessyoulike_2016-06-23",
          "_type" : "userdata",
          "_id" : "AVWGWKXO4q6RF88kI92F",
          "_score" : 12.930894,
          "_source" : {
            "date" : "2016-06-23 09:48:58.0",
            "flightPC" : 0,
            "uid" : "14BFADB2-4A60-7A50-C892-A75B35DB1A5B",
            "hotelPC" : 0,
            "ticketPC" : 0,
            "vacationPC" : 0,
            "id" : 64839549,
            "trainBook" : 0
          }
        } ]
      }
    }

请求：

    curl -Xpost 'http://10.90.4.44:10201/_bulk' -d '
    {"create":{"_index":"guessyoulike_2016-05-23","_type":"userdata"}}
    {"id":64772021,"uid":"000804C7-D87F-92CA-9043-45F594E7CCF8"}
    {"create":{"_index":"guessyoulike_2016-05-23","_type":"userdata"}}
    {"id":64772022,"uid":"0009E455-0DDB-83EC-742E-62526F65D257"}
    {"create":{"_index":"guessyoulike_2016-05-23","_type":"userdata"}}
    {"id":64772023,"uid":"000B92C8-62C4-C9FE-752C-7F7FCE6B462F"}
    {"create":{"_index":"guessyoulike_2016-05-23","_type":"userdata"}}
    {"id":64772024,"uid":"000C4634-4F36-DF83-9639-2B7B7320E686"}
    {"create":{"_index":"guessyoulike_2016-05-23","_type":"userdata"}}
    {"id":64772025,"uid":"000C6428-A1B5-B80F-3280-656073880070"}
    '

运行结果：

    {
        "took": 82,
        "errors": false,
        "items": [
            {
                "create": {
                    "_index": "guessyoulike_2016-06-23",
                    "_type": "userdata",
                    "_id": "AVWM0_HW4q6RF88kR-34",
                    "_version": 1,
                    "_shards": {
                        "total": 2,
                        "successful": 2,
                        "failed": 0
                    },
                    "status": 201
                }
            },
            {
                "create": {
                    "_index": "guessyoulike_2016-06-23",
                    "_type": "userdata",
                    "_id": "AVWM0_HW4q6RF88kR-35",
                    "_version": 1,
                    "_shards": {
                        "total": 2,
                        "successful": 2,
                        "failed": 0
                    },
                    "status": 201
                }
            },
            {
                "create": {
                    "_index": "guessyoulike_2016-06-23",
                    "_type": "userdata",
                    "_id": "AVWM0_HW4q6RF88kR-36",
                    "_version": 1,
                    "_shards": {
                        "total": 2,
                        "successful": 2,
                        "failed": 0
                    },
                    "status": 201
                }
            },
            {
                "create": {
                    "_index": "guessyoulike_2016-06-23",
                    "_type": "userdata",
                    "_id": "AVWM0_HW4q6RF88kR-37",
                    "_version": 1,
                    "_shards": {
                        "total": 2,
                        "successful": 2,
                        "failed": 0
                    },
                    "status": 201
                }
            },
            {
                "create": {
                    "_index": "guessyoulike_2016-06-23",
                    "_type": "userdata",
                    "_id": "AVWM0_HW4q6RF88kR-38",
                    "_version": 1,
                    "_shards": {
                        "total": 2,
                        "successful": 2,
                        "failed": 0
                    },
                    "status": 201
                }
            }
        ]
    }

1.**简介**  
 ES是一个高扩展的、开源的、全文检索的搜索引擎，它提供了近实时的索引、搜索、分析功能，系统是在Apache Lucene的基础上采用Java实现的。Lucene非常复杂，而ES通过RESTful API屏蔽Lucene的复杂性，提供了方便的应用接口。

2.**安装**    
从http://elasticsearch.org/download/下载，解压。最新版本需要jdk1.8，在beta机上需要在启动脚本里添加export java_home到1.8的目录。执行./bin/elasticsearch就可以运行。  
测试：

    curl 'http://localhost:9200/?pretty'

正常启动时的返回结果：

    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",   //集群名
    "version" : {
    "number" : "2.3.3",
    "build_hash" : "218bdf10790eef486ff2c41a3df5cfa32dadcfde",
    "build_timestamp" : "2016-05-17T15:40:04Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.0"
    },
    "tagline" : "You Know, for Search"
    }

多个ES实例通过相同的集群名构成一个集群，修改config/目录下的elasticsearch.yml文件即可修改cluster_name。

主要目录：  
bin：运行ES实例和插件管理所需要的脚本  
config：配置文件目录  
lib：ES使用的库  
data：ES使用的所有数据的存储位置  
logs：日志和错误记录文件  
plugins：安装的插件存储的位置  

3.**基本概念**  
*节点和集群*  
ES为了能够处理大数据集，实现容错和高可用性，它可以运行在许多互相合作的服务器上。这些服务器称为集群（cluster），形成集群的每个服务器称为节点（node）。  

*分片*  
当有大量的文档时，由于内存硬盘各方面的限制，单个节点可能不够。这种情况下，就需要将数据分为较小的称为分片的部分，每个分片可以放在不同的服务器上。当查询的索引分布在多个分片上时，ES会把查询发送给每个相关分片，并将结果合并。

*副本*  
每个分片可以有零个或多个副本

*索引*  
索引可以理解为关系数据库中的表。每个索引有一或多个分片。

*文档*  
存储在ES中的主要实体。相当于表中的一行记录。
