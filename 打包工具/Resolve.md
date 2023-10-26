使用 enhanced-resolve，webpack 能解析三种文件路径：
# 绝对路径 
import '/home/me/file';
import'C:\\Users\\me\\file';
由于已经获得文件的绝对路径，因此不需要再做进一步解析。
# 相对路径 
import '../src/file1';
import './file2';
在这种情况下，使用 import 或 require 的资源文件所处的目录，被认为是上下文目录。在 import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径。
# 模块路径 
import 'module';
import 'module/lib/file';
在 [resolve.modules](https://webpack.docschina.org/configuration/resolve/#resolvemodules) 中指定的所有目录检索模块。 你可以通过配置别名的方式来替换初始模块路径，具体请参照 [resolve.alias](https://webpack.docschina.org/configuration/resolve/#resolvealias) 配置选项。

- 如果 package 中包含 package.json 文件，那么在 [resolve.exportsFields](https://webpack.docschina.org/configuration/resolve/#resolveexportsfields) 配置选项中指定的字段会被依次查找，package.json 中的第一个字段会根据 [package导出指南](https://webpack.docschina.org/guides/package-exports/)确定 package 中可用的 export。

一旦根据上述规则解析路径后，resolver 将会检查路径是指向文件还是文件夹。如果路径指向文件：

- 如果文件具有扩展名，则直接将文件打包。
- 否则，将使用 [resolve.extensions](https://webpack.docschina.org/configuration/resolve/#resolveextensions) 选项作为文件扩展名来解析，此选项会告诉解析器在解析中能够接受那些扩展名（例如 .js，.jsx）。

如果路径指向一个文件夹，则进行如下步骤寻找具有正确扩展名的文件：

- 如果文件夹中包含 package.json 文件，则会根据 [resolve.mainFields](https://webpack.docschina.org/configuration/resolve/#resolve-mainfields) 配置中的字段顺序查找，并根据 package.json 中的符合配置要求的第一个字段来确定文件路径。
- 如果不存在 package.json 文件或 [resolve.mainFields](https://webpack.docschina.org/configuration/resolve/#resolvemainfields) 没有返回有效路径，则会根据 [resolve.mainFiles](https://webpack.docschina.org/configuration/resolve/#resolvemainfiles) 配置选项中指定的文件名顺序查找，看是否能在 import/require 的目录下匹配到一个存在的文件名。
- 然后使用 [resolve.extensions](https://webpack.docschina.org/configuration/resolve/#resolveextensions) 选项，以类似的方式解析文件扩展名。

webpack 会根据构建目标，为这些选项提供合理的[默认](https://webpack.docschina.org/configuration/resolve)配置。
