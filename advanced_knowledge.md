## 在vue中操作什麼屬性，在接口中就要存在該屬性的key.

例如，vue頁面，會在初始化時，請求一個接口：


{
  products: [
    {
      name: 'a'
      comment: 'lalala'
    },
    {
      name: 'b'
      comment: 'kakaka'
    }

  ]
}

如果在接口中，這個comment 不存在，那麼我們在vue中怎麼操作，
都不會把 'comment'放到  products中去。

## 發送層次很深的json 格式的參數


vuejs - 發送大量的請求時，使用 post + JSON.stringify(xx)的形式

2017-11-26 14:13
訪問量: 1

分類： 技術
如題，我們從vuejs向接口發起請求時， 這樣：

      this.$http.post(this.$configs.api + 'dish_orders/native_add_dish_order', {
        dishes: JSON.stringify(this.chosen_dishes),
      }).then((response) => {

服務器端會看到這樣的日誌：

{"socket_ips"=>"[{\"socketIPAddress\":\"192.168.1.181\",\"port\":9100,\"printType\":1,\"ticketTitle\":\"皇后鎮餐廳總單\"
,\"categoryArray\":[1,2,3,4,5,6,11,12,13],\"categoryHashArray\":[{\"id\":1,\"name\":\"特色菜\"},
{\"id\":2,\"name\":\"涼菜\"},{\"id\":3,\"name\":\"熱菜\"},{\"id\":4,\"name\":\"燒烤\"},
{\"id\":5,\"name\":\"麪點、主食\"},{\"id\":6,\"name\":\"酒水飲料\"},{\"id\":11,\"name\":\"早點\"},
{\"id\":12,\"name\":\"鐵鍋燉\"},{\"id\":13,\"name\":\"餐具\"}]},
{\"socketIPAddress\":\"192.168.1.182\",\"port\":9100,
\"printType\":0,\"ticketTitle\":\"熱菜、涼菜、特色菜、燒烤\", .....

然後，我們在rails端，這樣解析：

dishes = ActiveSupport::JSON.decode(params[:dishes])
