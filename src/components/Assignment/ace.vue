<template>
  <div>
    <div
      class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded"
      :class="[color === 'light' ? 'bg-white' : 'bg-emerald-900 text-white']"
    >
      <div class="rounded-t mb-0 px-4 py-3 border-0">
        <div class="flex flex-wrap items-center">
          <div class="relative w-full px-4 max-w-full flex-grow flex-1">
            <h3
              class="font-semibold text-lg"
              :class="[color === 'light' ? 'text-black-700' : 'text-white']"
            >
              Questions: 1
            </h3>
          </div>
        </div>
      </div>
        <div class="flex content-center flex-wrap -mx-2 p-3 bg-grey rounded shadow-lg">
  <div class="flex w-1/2 px-2 py-2">
    <div class="p-4 rounded shadow-lg bg-white text-black border-b border-r border-grey-dark">
      
        <div class="text-black">
          <p>Given two sorted arrays nums1 and nums2 of size m and n respectively,return the median of the two sorted arrays.</p><p><br><strong>Example 1:</strong><br>&nbsp;Input: nums1 = [1,3], nums2 = [2]<br>&nbsp;Output: 2.00000<br>&nbsp;Explanation: merged array = [1,2,3] and median is 2.</p><p><br><strong>Example 2:</strong><br>&nbsp;Input: nums1 = [1,2], nums2 = [3,4]<br>&nbsp;Output: 2.50000<br>&nbsp;Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.</p><p><strong>&nbsp;Constraints:</strong></p><ul><li><strong>nums1.length == m</strong></li><li><strong>nums2.length == n</strong></li><li><strong>0 &lt;= m &lt;= 1000</strong></li><li><strong>0 &lt;= n &lt;= 1000</strong></li><li><strong>1 &lt;= m + n &lt;= 2000</strong></li><li><strong>-106 &lt;= nums1[i], nums2[i] &lt;= 106</strong></li></ul><p><br>&nbsp;</p><p><strong>Follow up: The overall run time complexity should be O(log (m+n)).</strong></p>
        </div>
      </div>
  </div>
  <div class="flex-grow w-1/2 px-2 py-2">
    <div class="p-4 rounded shadow-lg bg-white text-black border-b border-r border-grey-dark">
            <v-ace-editor
    v-model="code"
    @init="editorInit"
    lang="javascript"
    theme="github"
    style="height: 500px" />
     </div>
  </div>
</div>
    </div>
  </div>
</template>

<script>
import { VAceEditor } from 'vue3-ace-editor';
import { VAceEditor } from 'vue3-ace-editor';

export default {
  data() {
    return {
      code: 'console.log("aaa")'
    }
  },
  components: {
    VAceEditor
  }
}
</script>