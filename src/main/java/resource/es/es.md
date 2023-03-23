添加索引
PUT index/type/_mapping
{
"properties": {
"wash_7d22_aging_achievement_quantity": {
"type": "integer"
},
"wash_aging_achievement_quantity": {
"type": "integer"
}
}
}





{
"query":{
"bool":{
"must":[
{
"range":{
"stat_date":{
"gt":"2023-03-01",
"lt":"2023-03-05"
}
}
},
{
"match":{
"business_type_id":"3"
}
},
{
"range":{
"wash_aging_achievement_quantity":{
"gt":"-1",
"lt":"5"
}
}
}
]
}
},
"from":0,
"size":10,
"sort":[

    ],
    "aggs":{

    }
}