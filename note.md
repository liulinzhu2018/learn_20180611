JimdbPromoWareidClient类，包括初始化jimdb client和从jimdb中获取query key的返回结果。
./search_frame/PromoWord/JimdbPromoWareidClient.cpp

PromoWareidListCache类，包含一个JimdbPromoWareidClient类的集合，进行统一管理。
Initialize()实例化JimdbPromoWareidClient对象
GetPromoProductDoclist() 传入query key, 从jimdb获取wareid列表，再根据wareid得到docid, vector<QueryRsItem> &result存放结果集,QueryRsItem包括wareid, docid, weight(=结果数量-当前的序号, 也就是说eg, 3个结果，weight分别就是3，2，1).

./search_frame/PromoWord/PromoWareidListCache.cpp：64 ？
