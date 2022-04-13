这是一个C++序列化工具.
这个工具对于任何来说是免费的, 因为代码难以从其它的项目中分离....

这个工作试图使用C++格式的结构定义C++并自动生成读写代码.

这个工具非常容易使用.
1, 下载执行文件(spf4stl.exe)和模板文件(*.tpt, spf4stl.tpt是生成文件的主要模板文件.)
2, 定义一个C++样式的结构(msg.v0是一个示例)
3, 运行spf4stl.exe 结构文件 会输出生成代码在标准输出窗口或者
    spf4st.exe 结构文件 -o 文件路径                     输出文件到指定路径

4, 添加生成的文件到您的项目文件中, 使用pk/uk之类的函数进行序列化和反序列化.
      
      代码用法示例:
        string ps = m.pk();
        msg o;
        o.uk(ps.c_str(), ps.size());
        m.pk_to("msg.raw_1");
        o.pk_to("msg.raw_2");
        msg fm;
        fm.uk_from("msg.raw_1");
        fm.pk_to("msg.raw_3");
        




spf文件结构
spf是sct的文件解析格式, 它是一种严格定义的旨在定义交换信息的格式.它使用C的结构体样式，增加了一点点额外内容。
它使用/**/和/样式注释

下面是一个带着注释的简单的格式定义示例

ver           = 0;        //spf版本号，当前只支持0。

#include "msg.v0"         //这是一个可选的包含段内容，用于生成兼容于以前版本的协议。

struct msg(magic=0xffaabb/*魔法字内容可选*/,ver=1/*版本可选，默认为0，只存在于根结构中有效*/){
  bool _b;//基础类型成员(类型包括: bool, i08, u08, i16, u16, i32, u32, i64, u64, float, double, string).
  u08 a(0:undefined, 1:well, 2:bad; undefined); //生成static const的成员，可以当作枚举类型使用，最后一个为初始化默认值，只对整形变量有效.
  u32 b[100]; //数组示例
  
  struct newstruct{ u32 a; }; //子结构, 不含魔法字和版本.
  struct ssss{ u64 b; };

  vector<map<string,deque<newstruct>>> ccn;   //c++不同容器.
  
};



spf假定我们会在不同阶段定义不同的协议，每个协议版本从v0,v1,v2...
您可以通过include来兼容以前的版本，或者您也可以只把它当作序列化工具。

特性
spf几乎支持所有的C++类型，包括string和数组(但不包括指针)
spf包括几乎所有的支持遍历的C++容器(包括, vector, list, deque, set, map, multiset, multimap).
spf支持嵌套的子结构和容器
