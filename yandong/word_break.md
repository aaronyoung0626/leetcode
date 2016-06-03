#####Word Break I & Word Break II
###��Ŀ��ַ��

[word break II](https://leetcode.com/problems/word-break-ii/)

[word break](https://leetcode.com/problems/word-break/)

###��Ŀ���⣺
����һ���ַ���S��һ���ֵ�dict���ж�S�ܷ��ֵ��ڵ��ַ��������ɣ�������ԵĻ�����true(Word break II��Ҫ����������ֵ)
###����˼·��
��һ��˼·��dfs

������
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
������C++

�ڶ���˼·��
��̬�滮��˼��
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
dfs�������ؽ���ע������dfs����٣����Կ��ܻ�����ظ���������Σ���cache
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
