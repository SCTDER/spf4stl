# spf4stl
this is a serializable tools for c++.  

this tools is free for any user, but now we cant split code from other project..  


the tool is a use define a struct like c++, and auto generate serializable code.  


----

# it is very easy.  

1, download the execute file(spf4stl.exe) and template file(*.tpt, the spf4stl.tpt the main template for generated files.)  

2, define a struct like c(the msg.v0 is a sample)  

3, launch spf4stl.exe msg.v2(the file you defined) output code to stdout or use  

    spf4st.exe msg.v2 -o msg.h                     output code to file msg.h  
    

4, add the code in your project, use pk/uk/... to pack as string for unpack from string.  

      
      like followed:  


        string ps = m.pk();
        msg o;
        o.uk(ps.c_str(), ps.size());
        m.pk_to("msg.raw_1");
        o.pk_to("msg.raw_2");
        msg fm;
        fm.uk_from("msg.raw_1");
        fm.pk_to("msg.raw_3");


-----

# the spf's struct  

the spf is sct's potocol format, is a strick format for define exchanged message.it is like a c++ struct, and add a little content.  

it use /**/ and // as comment like c++.  


here is a tiny sample with comment.  

```

ver           = 0;        //proto format parse version, now must 0.  


#include "msg.v0"         //this is optional, only for compatible with previous version.  


struct msg(magic=0xffaabb/*the magic is optional*/,ver=1/*the ver is optional and 0 is default, only work in main' struct*/){  
  bool _b;//the basic type include(bool, i08, u08, i16, u16, i32, u32, i64, u64, float, double, string).
  u08 a(0:undefined, 1:well, 2:bad; undefined); //this code will generate const static member named well/bad/.., and the member default is undefined, only work for integer.
  u32 b[100]; //array
  
  struct newstruct{ u32 a; }; //substruct, ignore magic, and version.
  struct ssss{ u64 b; };

  vector<map<string,deque<newstruct>>> ccn;   //c++ contains with diffrent types.
  
};

```



the spf assumpt your will defined diffrent version struct for diffrent progress.
your can defined as v0,v1,v2..., also you can ignore the version, and only as general serializable tools.

# feature
the spf store in binary format

the spf support almost c++ basic type,include array and string(except pointer).  

the spf support almost c++ contains(include, vector, list, deque, set, map, multiset, multimap, some contain cant traverse so not support).  

the spf support nest struct and contains.
