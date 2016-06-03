#####Word Break I & Word Break II
###题目地址：

[word break II](https://leetcode.com/problems/word-break-ii/)

[word break](https://leetcode.com/problems/word-break/)

###题目大意：
给定一个字符串S和一个字典dict，判断S能否被字典内的字符串所构成，如果可以的话返回true(Word break II需要输出这个最优值)
###解题思路：
第一种思路：dfs

···
class Solution {
public:
    bool wordBreak(string s, unordered_set<string>& wordDict) {
        if(cache.find(s) != cache.end()) return false;
        if(wordDict.find(s) != wordDict.end()) return true;
        for(int i = 0;i<s.length();++i){
            string tmp = s.substr(0,i+1);
            if(wordDict.find(tmp) != wordDict.end() && wordBreak(s.substr(i+1),wordDict)){
                return true;
            }
        }
        cache.insert(s);
        return false;
    }
private:
    unordered_set<string> cache;
};
···C++

第二种思路：
动态规划的思想
```
class Solution {
public:
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
       int slen = s.length();
       vector<int> dp(slen+1,0);
       dp[0] = 1;
       for(int i = 1;i<=slen;++i){
            for(int j = i-1;j>=0;--j){
                if(dp[j]){
                    string tmp = s.substr(j,i-j);
                    if(wordDict.find(tmp) != wordDict.end()){
                        dp[i] = 1;
                        break;
                    }
                }
            }
       }
       vector<string> ret;
       if(dp[slen]==0) return ret;
       build(ret,"",0,s,wordDict,dp);
       return ret;

    }
    void build(vector<string> &ret,string cur,int idx,string &s,unordered_set<string>& wordDict,vector<int> &dp){
        int slen = s.length();
        if(idx >= slen){
            if(cur.length() > 0 && cur[cur.length()-1] == ' '){
                cur = cur.substr(0,cur.length()-1);
            }
            ret.push_back(cur);
        }
        for(int i = idx+1;i<s.length();++i){
            if(dp[i+1]){
                string tmp = s.substr(idx,i-idx + 1);
                if(wordDict.find(tmp) != wordDict.end()){
                    build(ret,cur + " " + tmp,i+1,s,wordDict,dp);
                }
            }
        }
    }
};
```C++
dfs过程中重建，注意由于dfs是穷举，所以可能会出现重复计算的情形，加cache
class Solution {
public:
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        if(cache.find(s) != cache.end()) return cache[s];
        vector<string> ret;
        for(int i = 0;i<s.length();++i){
            string tmp = s.substr(0,i+1);
            if(wordDict.find(tmp) != wordDict.end()){
                vector<string> right = wordBreak(s.substr(i+1),wordDict);
                for(int j = 0;j<right.size();++j){
                    ret.push_back(tmp + " " + right[j]);
                }
            }
        }
        cache[s] = ret;
        return ret;
    }
private:
    unordered_map<string,vector<string>> cache;
};
