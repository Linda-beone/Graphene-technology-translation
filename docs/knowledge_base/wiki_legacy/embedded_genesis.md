  **原文链接**：[https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/embedded_genesis.html](https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/embedded_genesis.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

egenesis
====================

嵌入式创始
-----------------------

编译引用特定创世状态的可执行文件。 可以通过与`egensis_full`链接来包含完整的创世状态，或者通过与`egenesis_brief`链接可以仅包括哈希值（链ID和哈希的JSON）。

`GRAPHENE_EGENESIS_JSON`参数指定要包含的`genesis.json`。 注意，你必须用以下命令删除你的`cmake`残余

        make clean
        rm -f CMakeCache.txt
        find . -name CMakeFiles | xargs rm -Rf

嵌入式数据可以通过`egenesis.hpp`中的函数访问。

注意，如果你的`enesis.json`包含换行符，你应该知道有[换行符转换问题]（https://github.com/cryptonomex/graphene/issues/545）并且应该记住这个建议：

- 如果您正在创建新链，请在发布之前通过`canonical_format.py`运行你的创始文件。 它应该摆脱行转换问题，并具有使文件更小的额外好处。
- 如果你正在创建一个新的链，并且想在Github中显示一个带有换行符和空格的漂亮的创始文件，你可以通过运行`canonical_format.py`编辑`CMakeLists`来创建难看的文件。 如果你这样做，你的构建将依赖于一个有效的Python安装（这就是为什么我们不这样做）。 
- 如果你有一个具有漂亮创始链，或者想要忽略上述建议并创建一个具有漂亮的新链，并且你使用Git来分发你的创始文件，你应该添加一个`.gitattributes`文件到如这里所讨论的存储库。