``` cpp;
class Solution {

public:

    int longestConsecutive(vector<int>& nums) {

        std::unordered_set<int> set(nums.begin(), nums.end());

        int longest = 0;

  

        for(int num : set){

            if(!set.count(num-1)){

                int currentNum = num;

                int currentLength = 1;

  

                while(set.count(currentNum+1)){

                    currentNum++;

                    currentLength++;

                }

                longest = max(longest,currentLength);

            }

        }

        return longest;

    }

};
```
## Key notes
- initializing hash sets:
		std::unordered_set<> set(vec.begin(), vec.end())
- set.count(n) returns a boolean whether or not an element exists
- Why a hash set:
	- order does not matter,
	- repetition does not matter
	- we only care about whether or not an element exists. 