


主要在包装的时候对图书的各种数据进行汇总，代码位置 query_result_json_packer.cpp  QueryResultPacker::packAttr()

主要流程



在图书垂直化页面做折扣和星级汇总
if((ctxt.attr_coll_flags.coll_flags.extattr_coll && optimum_selected) && (analysisDat->m_vcate_type == MasterServerCore::QS_VCATE_LIST))
1. 按折扣区间汇总
代码所在位置 collection/bookattrs_collection.h 里面 DiscountCollection函数
用途：根据所有召回结果，汇总出现的折扣。
折扣区间分为：[1,3),[3,5),[5,7),[7,10]
折扣计算方法：折扣 = 100*京东价/市场价
（其中京东价和市场价取的是正排字段，
配置位置server.ini,
[SERVER_FIELD] 
field.wprice_field=wredisprice 市场价, 
field.hprice_field=dredisprice 京东价）

2. 按星级汇总
代码所在位置 collection/bookattrs_collection.h 里面 StarLevelCollection函数
用途：根据所有召回结果，汇总星级。
星级范围：1~5，从正排字段取星级，
配置位置server.ini 
[SERVER_FIELD] 
field.starlevel_field=averagescore

top分类是图书时只在垂直化页面做价格汇总

3. 按价格汇总(top分类图书垂直页面)
100一个档，当价格区间>50个时，；当价格区间>7 且< 50 时，；

server.field.filter_by_sort_type_on_cid=m_show_cid:m_show_chld_cid 左侧展示的分类

价格字段，server.ini
[SERVER_FIELD] 
field.price_field=xredisprice


4. 对属于虚拟图书类型的分类汇总类型是否展示进行调整（垂直化页面，且仅限图书分类&音像分类）
代码位置CategoryCollectionShowAdjust() category_collection.cpp
（1）当有对分类的汇总且有url的cid3集合（从json结果集文件中获取url对应的cid3），且url传入的需要展示的cid2,cid3都在分类汇总中时，需要调整展示分类汇总。
（2）遍历url对应的cid3的map获取cid3，和根据分类树获取到对应该cid3的父分类cid2，获取所有cid2属于虚拟图书类型的cid2,cid3集合.
（3）对分类汇总结果中需要展示的cid2和cid3集合按照cid权重排序
（4）对排序后的前6个cid2进行下面处理，如果需要展示的cid2属于虚拟图书，且它对应的child cid（cid3）也属于虚拟图书类型，那么只要不超过配置中的子分类展示个数就展示这个cid3,cid2
子分类展示个数在server.ini中[SERVER_ENV] server.env.collection_child_show_num 默认是2
（5）按照是否展示对cid2和cid3集合进行排序。

5. 根据op_filter的虚拟分类过滤的配置
当虚拟分类类型在分类汇总中，且有op_filter的虚拟分类类型也在分类汇总中时，
1 == vir_cat_filt_type， 虚拟分类类型的分类都不展示
2 == vir_cat_filt_type，虚拟分类类型的分类在op的虚拟分类类型的黑名单的不展示
去掉所有不展示的分类
 

6. 装帧&包装汇总（仅限top分类，图书分类&音像分类），出版社汇总，是否套装汇总（仅限top分类，且是图书分类）
都是取正排字段，调用NumberStatis::DoStatis()函数 business/pack/number_statis.cpp 进行汇总。

[SERVER_FIELD]
field.package_field=package
field.publishers_field=publishers
field.packState_field=packstate


6. 包装图书的星级和折扣扩展属性时，都是多余一个才进行包装。
query_result_json_packer.cpp void QueryResultPacker::packJsonBookAttrs()


排序前调用排序模块得到的结果集的高相关分类类型top_cate_type和虚拟分类类型virt_cate_type，代码在master_server.cpp void MasterServerCore::SortResults()

高相关分类类型:
图书和音像分类在汇总和包装图书的时候有用到。
enum
{
  QS_TOP_CATE_UNKNOWN = 0, 				// 未知
  QS_TOP_CATE_3C = 0x1,					// 3C
  QS_TOP_CATE_RIBAI = 0x2,				// 日百
  QS_TOP_CATE_BOOK = 0x4,				// 图书
  QS_TOP_CATE_AUDIOVIDEO = 0x8,			// 音像
  QS_TOP_CATE_CLOTHINGSHOE = 0x10,		// 衣服，鞋 
  QS_TOP_CATE_SHOPALTERNATE = 0x20,		// shop alternate
  QS_TOP_CATE_NOT_SHOPALTERNATE = 0x40	// no shop alternate
};

虚拟分类类型
enum
{
  QS_VCATE_NONE = 0,
  QS_VCATE_MERGE = 0x1,	// 进行主从合并
  QS_VCATE_SIZE = 0x2,	// 尺码汇总
  QS_VCATE_LIST = 0x4,  // 垂直化
  QS_VCATE_LONG = 0x8,  // 长图展示
  QS_VCATE_AGE = 0x10,  // 年龄
  QS_VCATE_COLOR = 0x20 // 颜色汇总
};


need_optimum_selecting 是否需要非默认排序过滤
满足url传入的exp_keys参数包含server.ini中配置的expand_attr_field字段,或是满足url有sort_type且不是默认类型，或是满足url中有各种属性参数(brand,color,size,price)时，需要非默认排序need_optimum_selecting=true
代码位置./src/searcher/src/business/analysis_context.cpp void AnalysisContext::Init()



AnalysisContext::Init()
结合配置文件的配置，分析url参数
1. 根据url filt_type中是否包含一二级分类字段cid1, cid2，判断是否点击分类
server.ini中配置cid1,cid2
[SERVER_FIELD]
server.field.filter_by_cid1=cid1
server.field.filter_by_cid2=cid2
2. 根据url expand_key 判断是否有扩展结果
3. 根据server.ini中server.env.none_default_sort_filter，和url扩展属性是否包含配置文件中配置的扩展字段或是url中sort_type!=sort_default，判断是否非默认排序过滤
4. 不展示属性的情况：
		a. 搜索词属于特殊词，且这个特殊词未配置属性展示，未点击分类时
		b. op过滤不展示属性时
   展示属性的情况：
		a. url中带有强制归总参数时
		b. 其他情况都展示
5. 根据url判断价格、品牌、尺寸、价格等其他属性是否汇总


beforePack
1. 非默认排序结果过滤，NoneReleRankFilter
2. 根据新品字段判断是否结果集中有新品
3. 合并一品多商
4. FilterSKU
5. MergeSKU
6. 标记货到付款信息MarkCashOnDelivery
7. 主从商品合并之后，再次排序，调用排序模块
8. 山姆商品逻辑，同一termid下只保留一个最优商品

packAttr
1. url标记为o2o且有经纬度参数时，包装最近的门店id和距离
2. url标记为命中merger cache时，直接返回
3. CategoryCollectionAll
4. 品牌、颜色、鞋码汇总
5. 运营结果归总OpPromotionCollection
6. 图书汇总
7. 数值类型归总
8. 京东配送时效汇总
9. 发货地汇总
10.各种归总结果进行包装




特殊词来自server.ini [FILE_LIST] server.file.special_keyword_file = $(server.data_path)$/main/txtdata/special_keyword.txt


nohup ./article.release.log > /dev/null 2>&1 &
lsof -p 36276 | grep REG | grep -v event
vim set nonu
JimdbPromoWareidClient类，包括初始化jimdb client和从jimdb中获取query key的返回结果。
./search_frame/PromoWord/JimdbPromoWareidClient.cpp

PromoWareidListCache类，包含一个JimdbPromoWareidClient类的集合，进行统一管理。
Initialize()实例化JimdbPromoWareidClient对象
GetPromoProductDoclist() 传入query key, 从jimdb获取wareid列表，再根据wareid得到docid, vector<QueryRsItem> &result存放结果集,QueryRsItem包括wareid, docid, weight(=结果数量-当前的序号, 也就是说eg, 3个结果，weight分别就是3，2，1).

./search_frame/PromoWord/PromoWareidListCache.cpp：64 ？
