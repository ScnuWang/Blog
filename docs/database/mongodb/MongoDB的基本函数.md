ProductModel.objects.limit(取多少条数据)

关键字查询：product_list = ProductModel.objects(project_name__contains='马克杯')

排序：在 MongoDB 中使用 sort() 方法对数据进行排序，sort() 方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而 -1 是用于降序排列。 

