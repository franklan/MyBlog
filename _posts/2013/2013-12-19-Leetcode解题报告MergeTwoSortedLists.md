---
layout: default
title: Leetcode解题报告Merge Two Sorted Lists
---

## 题目描述 ##
### Simplify Path （*Diff 2 Freq 5*）###
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

## 代码 ##

    
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     * int val;
     * ListNode *next;
     * ListNode(int x) : val(x), next(NULL) {}
     * };
     */
    class Solution {
    public:
  	  ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
    	if(l1 == NULL)
		    return l2;
    	if(l2 == NULL)
    		return l1;
    	ListNode* pHead = NULL;
    	if(l1->val < l2->val)
    	{
    		pHead = l1;
    		pHead->next = mergeTwoLists(l1->next,l2);
    	}
    	else
    	{
    		pHead = l2;
    		pHead->next = mergeTwoLists(l1,l2->next);
    	}
    	return pHead;
      }
    };


## 结果 ##

    208 / 208 test cases passed.
    Status: Accepted
    Runtime: 52 ms
    Submitted: 0 minutes ago
    