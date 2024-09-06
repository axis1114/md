# gingorm笔记

### gin的3种模式

##### 1.debug模式

调试模式，这是Gin的默认模式。在调试模式下，Gin会输出更多日志信息，例如路由匹配信息、请求参数、响应内容等，对性能有一定影响。

##### 2.**Release 模式**

发布模式。在发布模式下，Gin只会输出必要的日志信息，例如错误信息和警告信息，可以提高应用程序的性能。

##### 3.Test 模式

测试模式。在测试模式下，Gin会提供一些额外的工具和功能来帮助测试人员进行测试。

### 日志

gin框架可自定义Logger 和 Recovery 中间件，一般来说一个项目有两个日志配置：**全局日志**和**gin框架日志**

### 	参数绑定

##### 1.ShouldBind

使用`c.ShouldBind`方法时，Gin 框架会根据请求的 Content-Type 自动选择适当的数据绑定方法（如 JSON、XML 等）。

##### 2.ShouldBindJSON

```go
type User struct {
    Username string `json:"username"`
    Age      int    `json:"age"`
}
```



##### 3.ShouldBindQuery

```go
type User struct {
    Username string `form:"username"`
    Age      int    `form:"age"`
}
```

##### 4.ShouldBindHeader

```go
type Header struct {
	SignatureKey string `form:"signaturekey"`
	Version      string `form:"version"`
	UserAgent    string `form:"User-Agent"`
}
```

##### 5.ShouldBindUri

```go
type User struct {
	ID   string `uri:"id"`
}
```

### 路由组

```go
type RouterGroup struct {
	*gin.RouterGroup
}

apiRouterGroup := router.Group("api")
routerGroupApp := RouterGroup{apiRouterGroup}

routerGroupApp.SettingsRouter()

func (router RouterGroup) SettingsRouter() {
	settingsApi := api.ApiGroupApp.SettingsApi
    router.GET("settings/site", settingsApi.SettingsSiteInfoView)
}
```

### gorm中的多对多

```GO
type ArticleModel struct {
  ID    uint
  Title string
  Tags  []TagModel `gorm:"many2many:article_tags;joinForeignKey:ArticleID;JoinReferences:TagID"`
}

type TagModel struct {
  ID       uint
  Name     string
  Articles []ArticleModel `gorm:"many2many:article_tags;joinForeignKey:TagID;JoinReferences:ArticleID"`
}

type ArticleTagModel struct {
  ArticleID uint `gorm:"primaryKey"`
  ArticleModel ArticleModel `gorm:"foreignKey:ArticleID"`
  TagID     uint `gorm:"primaryKey"`
  TagModel TagModel `gorm:"foreignKey:TagID"`
  CreatedAt time.Time
}
```

