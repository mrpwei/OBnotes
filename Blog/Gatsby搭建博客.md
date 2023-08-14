## 官网教程

[The Fastest Frontend for the Headless Web | Gatsby (gatsbyjs.com)](https://www.gatsbyjs.com/)
[Tutorial | Gatsby (gatsbyjs.com)](https://www.gatsbyjs.com/docs/tutorial/)

官网这个博客数据大都是基于frontmatter的，但是个人写markdown的时候不喜欢写frontmatter，因此数据要从其他地方获取，比如文章title就是markdown文件名，网址slug也可以通过title来转换。

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303141130817.png)

Notice that the `name` field in the `allFile` returns the same file names as the `parent` in the `allMdx`. This happens because when the transformer plugin creates a new `MDX` node from the `File` node, it allows us to access to the data of the parent `File` node via the `parent` field.

#todo
有的数据获取很麻烦，或者需要自定义，这时我们可以自己给nodes添加数据项

[gatsby-plugin-mdx | Gatsby (gatsbyjs.com)](https://www.gatsbyjs.com/plugins/gatsby-plugin-mdx/#extending-the-graphql-mdx-nodes)

[Dynamically generate Mdx pages based on file name | Aamnah - Frontend developer](https://aamnah.com/gatsby/gatsby_pages_mdx_generate_slugs)
[Don’t use frontmatter to seperate your markdown files in GatsbyJS - Use the file system - George Nance](https://georgenance.com/dont-use-frontmatter-markdown-files-gatsby)

[GraphQL query for the directory tree · Issue #9630 · gatsbyjs/gatsby (github.com)](https://github.com/gatsbyjs/gatsby/issues/9630)
https://github.com/meetup/swarm-design-system/blob/master/gatsby-node.js#L5-L35

- [x] hello
- [x] 📅 2023-08-14 