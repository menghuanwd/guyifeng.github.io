---
layout: post
title:  "Elasticsearch Geo Search"
date:   2019-07-10 17:03:58 +0800
# categories: 
tags: [golang, elasticsearch]
---
golang 使用 github.com/olivere/elastic 来进行经纬度搜索，范围查询

{% highlight golang linenos %}
package services

import (
	"context"
	"data-dict-api/dao"
	"github.com/olivere/elastic"
)

// SearchGeoParams ...
type SearchGeoParams struct {
	Model    interface{}
	Index    string
	DocType  string
	Lat      float64
	Lon      float64
	Distance string
}

func SearchGeoList(searchGeoParams SearchGeoParams) (*elastic.SearchResult, error) {
	ctx := context.Background()

	q := elastic.NewGeoDistanceQuery("location").Distance(searchGeoParams.Distance).Lat(searchGeoParams.Lat).Lon(searchGeoParams.Lon)

	sorter := elastic.NewGeoDistanceSort("location").Point(searchGeoParams.Lat, searchGeoParams.Lon).Order(true).Unit("km").SortMode("min")

	boolQ := elastic.NewBoolQuery()
	boolQ.Must(elastic.NewMatchAllQuery())
	boolQ.Filter(q)

	searchResult, err := dao.ESClient.Search(searchGeoParams.Index).Type(searchGeoParams.DocType).Size(9999).Query(boolQ).SortBy(sorter).Do(ctx)

	if err != nil {
		return nil, err
	}

	return searchResult, nil
}

{% endhighlight %}
