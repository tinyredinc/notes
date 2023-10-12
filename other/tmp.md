# TMP DOC SHARE

## Request url

<https://red.sideai.com/tile/data/>

## Request Method

GET

## Request Param

- tile_keyword - optional, string, 关键词模糊匹配
- tile_sku - optional, string, 瓷砖单品唯一编码
- tile_size - optional, string, 瓷砖尺寸, 有固定选项
- tile_color - optional, string, 瓷砖颜色, 有固定选项
- tile_finish - optional, string, 饰面处理, 有固定选项
- tile_location - optional, string, 存放位置编码
- stock_level - optional, int, 库存量平尺数(大于)多少

## Response Sample

```TEXT
GET https://red.sideai.com/tile/data/?tile_size=12X24&tile_keyword=NOVA
```

```JSON
[
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/dc3b7690-4f5f-4ca3-aeca-b02d003a006f.jpg",
        "tile_sku": "60",
        "tile_name": "BOXES - CASA NOVA FORTE 12X24 PANARIA (16 SQFT\\/BOX) (1 SKID) (7B13)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "32",
        "stock_skid": "1.0",
        "sqft_price": "3"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/449cc1be-f090-4d2c-b2d4-b02d003a1689.jpg",
        "tile_sku": "321",
        "tile_name": "BOXES - NOVA BROWN 60009 NATURAL 12X24 GRANITTOTUV (16 SQFT\\/BOX) (1 SKID) (7B9)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "40",
        "stock_skid": "1.0",
        "sqft_price": "2"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/7da9da62-9e02-4948-94cb-b02d003a178b.jpg",
        "tile_sku": "322",
        "tile_name": "BOXES - NOVA BROWN 60009 NATURAL 12X24 GRANITTOTUV (16 SQFT\\/BOX) (1 SKID) (7B7)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "40",
        "stock_skid": "1.0",
        "sqft_price": "2"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/fdcb1b40-b1dc-4ec0-ba7d-b02d003a1847.jpg",
        "tile_sku": "323",
        "tile_name": "BOXES - NOVA IVORY 60000 NATURAL 12X24 GRANITTOTUV (16 SQFT\\/BOX) (2 SKIDS) (7B19)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "79",
        "stock_skid": "2.0",
        "sqft_price": "3"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/4df6c7db-ff6d-4a73-a32b-b02a01006d7b.jpg",
        "tile_sku": "324",
        "tile_name": "BOXES - NOVA IVORY 60000L POLISHED 12X24 GRANITTOTUV (16 SQFT\\/BOX) (3.5 SKIDS) (7B19)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "140",
        "stock_skid": "3.5",
        "sqft_price": "3"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/4df6c7db-ff6d-4a73-a32b-b02a01006d7b.jpg",
        "tile_sku": "325",
        "tile_name": "BOXES - NOVA IVORY 60000L POLISHED 12X24 GRANITTOTUV (16 SQFT\\/BOX) (2 SKIDS) (7B8)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "80",
        "stock_skid": "2.0",
        "sqft_price": "4"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/39078b7c-2fb7-4810-a6aa-b02a01006cc5.jpg",
        "tile_sku": "326",
        "tile_name": "BOXES - NOVA NERO 60007 NATURAL 12X24 GRANITTOTUV (16 SQFT\\/BOX) (1 SKID) (7B19)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "40",
        "stock_skid": "1.0",
        "sqft_price": "3"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/5e154eee-0d32-4974-b1b0-b036010b7148.jpg",
        "tile_sku": "327",
        "tile_name": "BOXES - NOVA NERO 60007L POLISHED 12X24 GRANITTOTUV (16 SQFT\\/BOX) (1 SKID) (7B5)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "40",
        "stock_skid": "1.0",
        "sqft_price": "3"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/5d57ed06-44ab-4935-bee2-b02a01006c0a.jpg",
        "tile_sku": "328",
        "tile_name": "BOXES - NOVA VERDE 60003 NATURAL 12X24 GRANITTOTUV (16 SQFT\\/BOX) (2 SKIDS) (7B19)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "80",
        "stock_skid": "2.0",
        "sqft_price": "3"
    },
    {
        "img_src": "https:\\/\\/portal-images.azureedge.net\\/auctions-2023\\/danbur10139\\/images\\/e0266454-b49f-4a8d-8c59-b02a01006b2b.jpg",
        "tile_sku": "329",
        "tile_name": "BOXES - NOVA VERDE 60003 NATURAL 12X24 GRANITTOTUV (16 SQFT\\/BOX) (1 SKID) (7B5)",
        "tile_size": "12X24",
        "tile_width": "12",
        "tile_length": "24",
        "sqft_per_piece": "2.0",
        "sqft_per_box": "16.0",
        "stock_box": "40",
        "stock_skid": "1.0",
        "sqft_price": "3"
    }
]
```
