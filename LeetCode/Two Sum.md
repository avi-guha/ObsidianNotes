``` cpp;
class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {

        // Flow of program:

        /*

        1. add all the members of the vector based on index to a hash map

        2. We iterate through the vector. Each time we check target - nums.at(i) that is not the index of our current index i.

        3. if we find an index that satisfies this, we are good.

        */

        std::unordered_map<int,int> map;

        std::vector<int> a;

  

        for(int i = 0; i < nums.size(); i++){

            int newTarget = target - nums[i];

            if(map.count(newTarget)){

                auto it = map.find(newTarget);

                a = {it->second, i};

                return a;

            }

            map[nums.at(i)] = i;

        }

    return a;

    }

};
```
## Notes
- initializing a hash set std::unordered_map<X,X> map;
- ->: is a member access. dereference and access. 
- Dereference:
	- when we have pointers, it doesn't contain the value itself - it has the address of where that value lives in memory. 
	- Example:
	``` c++;
	int x = 10
	int*p = &x
	```
	- x is 10. 
	- p is a pointer to the address of memory that is 10. 
	- p points to where 10 lives in memory 
	``` c++;
	int y = *p
	```
	- Dereferencing means we are following the pointer to the thing it points to. 
		- This means y is now 10. We have followed the pointer now we dereference from that pointer. 
- Access: 
	- Once we have followed the pointer and reached the object, we can access its members (field / methods)
``` c++'
struct Point { int x; int y; };

Point p1 = {2, 3};
Point* ptr = &p1;
```
 - \*ptr is is pointing to the type of object Point stored in the memory p1
	 - (\*ptr).x means access the x field of that point 
- Using ptr->x is the same as doing (\*ptr).x 