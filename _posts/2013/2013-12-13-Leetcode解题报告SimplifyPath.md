---
layout: default
title: Leetcode解题报告Simplify Path
---


## 题目描述 ##
### Simplify Path（*Diff 3 Freq 1*） ###
Given an absolute path for a file (Unix-style), simplify it.

For example,
path = "/home/", => "/home"

path = "/a/./b/../../c/", => "/c"

Corner Cases:

Did you consider the case where path = "/../"?
In this case, you should return "/".

Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".
In this case, you should ignore redundant slashes and return "/home/foo".


刚开始写这道题时一直在尝试使用string.find解析path，解析出的文件夹名字保存到vector中，遇到/..就pop_back,最后一层循环打印即可。但是各种特殊情况让程序的判断增多，程序变得很复杂。后来发现，只要遍历字符，讨论几种情况就可以了。


## 代码 ##

    
    class Solution {
    public:
    	string simplifyPath(string path) 
		{
    			vector<string> pathName;
    			string seg="";
    			for(int i=0;i<=path.size();++i)
    			{
    				if(i==path.size() || path[i] == '/')
    				{
    					if(seg == "..")
    					{
    						if(!pathName.empty())
    							pathName.pop_back();
    					}
    					else if(seg == ".")
    						;
    					else if(seg.size()>0)
    					{
    						pathName.push_back(seg);
    					}
    					seg="";
    				}
    				else
    				{
    					seg+=path[i];
    				}
    			}
    			string res = "/";
    			for(int i=0;i!=pathName.size();++i)
    			{
    				if(i!=0)
    					res+="/";
    				res+=pathName[i];
    			}
    			return res;
    	}
    };

## 结果 ##

    252 / 252 test cases passed.
    Status: Accepted
    Runtime: 44 ms
    Submitted: 1 week, 5 days ago
