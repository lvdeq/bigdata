## spark介绍
1. 快如闪电的集群计算
2. 快速和通用的大规模数据处理技术
3. spark执行mr作业程序在内存比hadoop快100倍，磁盘上快10倍
4. spark有DAG(有向无环图)执行引擎，支持离散数据流和内存计算
5. 支持 java 、scala、python、R
6. spark有自己的集群计算技术：RDD

## spark部署的模式
1. standlone 独立模式
  在hdfs上分配空间，spark和mr同时运行，覆盖所有的job
2. 在yarn上运行，有助于spark和hadoop集成
3 spark in mr (hadoop v1)

## spark 组件
1. spark core (内核)
  内核位于执行引擎之上，所有功能都在其上构建，提供内存计算以及外部存储系统的数据集引用
  
2. spark sql
   在core之上引入的新的数据集抽象（schemaRDD）,支持结构和半结构数据    
3. spark streaming 执行流分析

4. MLLib
  机器学习
5. gaphx
  图处理
  
  
## spark安装
1. 下载spark文件
2. 配置conf/spark-env.sh
3.配置spark环境变量
4.执行spark-shell 进入 scala shell终端  
5.通过浏览器访问 spark web ui , ip:8080
6.可以修改spark日志级别

## sparkContext
sc是spark主入口，负责连接到spark cluster
sc用于创建RDD
### RDD 
弹性分布式数据集，不可变，可分区的数据集合
基本操作：map,filter,persist
RDD特征：
1. 有一个分区列表
2. 每个split都有个计算函数
3. 存放present依赖列表
4. 基于kv的分区器
5. 可选的位置列表

### RDD操作
执行spark-sell 进入终端，默认使用的是local模式，没有用到集群
```
val lines=sc.textFile("file:///usr/words.txt");
lines.count
lines.first
lines.take(2) //提取前两行
lines.map(x=>{
  println(x)  //打印每一行数据
  x
})

val words=lines.flatMap(x=>x.split(","))
lines.map(x=>(x,1)).reduceByKey((x,y)=>x+y)
```
spark-shell --master local[2] 2表示启动几个线程数，来模拟spark集群

### 独立的应用程序

```
SparkConf conf=new SparkConf();
conf.setMaster("local[4]");
conf.setAppName("SimpelApp");
JavaSparkContext context=new JavaSparpkContext(conf);
JavaRDD<String> rdd=context.textFile("d://words.txt");
JavaRDD<String> words=rdd.flatMap(new FlatMapFunction<String,String>(){
  public Iterable call(String strs){
      return Arrays.asList(strs.split(","));
  }
}   
)

//统计单词数量
JavaPairRDD counts=words.mapToPair(new PairFunction<String ,String, Integer>(){
    public Tuple2<String,Integer> call(String x){
      return new Tuple2(x,1);
    }
}).reduceByKey(new Function2<Integer,Integer,Integer>(){
    public Integer calll(Integer x,Integer y){
        return x+y;
    }
});

```



  
