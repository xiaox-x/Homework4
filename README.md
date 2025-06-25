# 北航软件需求与系统设计作业4

## 任务一——基于restful调用open API，进行内容生成
任务文件见python文件——任务一。
![image](https://github.com/user-attachments/assets/8c36db7b-8a6d-4373-91e8-d132d178b3a3)
### python截图
###运行结果

 
=== 博客系统设计方案 ===
=== 博客系统设计方案 ===
### 博客系统设计

#### 1. 用例图

在用例图中，我们识别用户和系统之间的交互以及不同用户类型的参与：

```
Actors:
- Visitor
- Registered User
- Author
- Administrator

Use Cases:
1. View Blog Posts (Visitor, Registered User)
2. Register (Visitor)
3. Login (Visitor, Registered User)
4. Create Blog Post (Author)
5. Edit Blog Post (Author)
6. Delete Blog Post (Author)
7. Comment on Blog Post (Registered User)
8. Moderate Comments (Administrator)
9. Manage Users (Administrator)
```

#### 2. 系统顺序图

系统顺序图展示系统处理特定任务的动态流程，例如创建博客文章的过程：

```
Actors: Author

Sequence:
1. Author initiates Create Blog Post
2. System prompts Author for blog details (Title, Content, Tags)
3. Author submits blog details
4. System validates blog details
5. System stores the new blog post in the database
6. System confirms successful creation of blog post to Author
```

#### 3. 概念类图

概念类图定义系统的主要对象以及它们之间的关系：

```
Classes:
- User
  - Attributes: userId, username, password, email
  - Operations: register(), login(), logout()

- BlogPost
  - Attributes: postId, title, content, authorId, createdDate, tags
  - Operations: createPost(), editPost(), deletePost()

- Comment
  - Attributes: commentId, content, postId, userId, createdDate
  - Operations: addComment(), deleteComment()

- Administrator
  - Attributes: adminId, permissions
  - Operations: moderateComments(), manageUsers()
```

Relationships:
- User 1..* creates BlogPost
- User 0..* writes Comment on BlogPost
- Administrator 0..1 moderates Comment

#### 4. OCL合约

OCL（对象约束语言）合约用于定义类的方法约束和不变条件。

```
context User::register()
post: self.userId <> null and self.email.isNotEmpty()

context BlogPost::createPost(title, content, authorId)
pre: title.isNotEmpty() and content.isNotEmpty()
post: BlogPost->includes(title) and BlogPost.authorId = authorId

context Comment::addComment(content, postId, userId)
pre: content.isNotEmpty()
post: Comment->includes(postId) and Comment.userId = userId

context Administrator::moderateComments(commentId)
pre: Comment->exists(commentId)
post: Comment->select(c | c.commentId = commentId).size() = 0
```


## 任务二——基于API SDK,进行内容生成
任务文件见python文件——任务二。api key已过期，需自行替换进行查看。
### python截图
![image](https://github.com/user-attachments/assets/c9a9204e-f7cd-45d4-8f9d-05b8c381943d)

### 运行结果
=== 博客系统设计方案 ===
### 博客系统设计模型

#### 一、用例图

在该博客系统中，我们识别出以下用例：

- 博主：发布博客、编辑博客、删除博客、审核评论。
- 读者：查看博客、评论博客。
- 系统管理员：管理用户、审核博客内容。

```
Use Case Diagram:

+-------------------------------------+
|            博客系统用例图            |
|-------------------------------------|
| Actor: 博主                          |
| - 发布博客                          |
| - 编辑博客                          |
| - 删除博客                          |
| - 审核评论                          |
|-------------------------------------|
| Actor: 读者                          |
| - 查看博客                          |
| - 评论博客                          |
|-------------------------------------|
| Actor: 系统管理员                    |
| - 管理用户                          |
| - 审核博客内容                      |
+-------------------------------------+
```

#### 二、系统顺序图

我们这里描述“发布博客”这一功能的顺序图：

```
Sequence Diagram for "发布博客":

+-------------+    +----------+          +------------+
|    博主     |    |  系统    |          |  数据库    |
+-------------+    +----------+          +------------+
      |                  |                     |
      |  输入内容        |                     |
      |----------------->|                     |
      |                  |    保存草稿        |
      |                  |------------------->|
      |                  |                     |
      |  提交博客        |                     |
      |----------------->|                     |
      |                  |  验证信息          |
      |                  |------------------->|
      |                  |                     |
      |                  |  成功发布博客      |
      |<-----------------|                     |
      |                  |                     |
```

#### 三、概念类图

概念类图描述主要实体及其关系：

```
Conceptual Class Diagram:

+----------------+          +-------------+          +-------------------+
|    用户 (User) |<>-------o|   博客 (Blog) |<>-----o|   评论 (Comment)   |
|----------------|          |-------------|          |-------------------|
| - 用户ID       |          | - 博客ID    |          | - 评论ID          |
| - 用户名       |          | - 标题      |          | - 评论内容        |
| - 密码         |          | - 内容      |          | - 评论者          |
+----------------+          | - 发表日期  |          | - 评论日期        |
                            | - 作者      |          +-------------------+
                            +-------------+
```

#### 四、OCL合约

确保系统功能正确的OCL合约示例：

```ocl
context Blog::publish(blog: Blog)
pre: self.userType = 'Author' and blog.content->notEmpty()
post: Blog.allInstances()->exists(b | b.title = blog.title and b.author = self)

context Comment::addComment(blog: Blog, commentContent: String)
pre: commentContent->notEmpty() and blog.allowComments = true
post: Comment.allInstances()->exists(c | c.content = commentContent and c.blog = blog)

context System::approveComment(comment: Comment)
pre: comment.status = 'Pending'
post: comment.status = 'Approved'
```


## 任务三——基于调用工作流生成用例图、类图、时序图和ocl合约
[博客系统设计文档.pdf](https://github.com/user-attachments/files/20907846/default.pdf)
### 内容生成
见下在线链接。or见上传的的文档《博客系统设计文档.pdf》
