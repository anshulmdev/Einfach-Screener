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
              Coding Questions
            </h3>
          </div>
        </div>
      </div>
      <div class="block w-full overflow-x-auto">
        <!-- Projects table -->
        <div class="grid grid-cols-2 gap-2">
  <div class="flex-1 relative py-3 sm:max-w-xl sm:mx-auto">
    <div class="relative pr-4 py-10 bg-white shadow-lg sm:rounded-3xl sm:p-20">
      <div class="max-w-md mx-auto">
        <div class="text-black">
          <p>Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.</p><p><br><strong>Example 1:</strong><br>&nbsp;Input: nums1 = [1,3], nums2 = [2]<br>&nbsp;Output: 2.00000<br>&nbsp;Explanation: merged array = [1,2,3] and median is 2.</p><p><br><strong>Example 2:</strong><br>&nbsp;Input: nums1 = [1,2], nums2 = [3,4]<br>&nbsp;Output: 2.50000<br>&nbsp;Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.</p><p><strong>&nbsp;Constraints:</strong></p><ul><li><strong>nums1.length == m</strong></li><li><strong>nums2.length == n</strong></li><li><strong>0 &lt;= m &lt;= 1000</strong></li><li><strong>0 &lt;= n &lt;= 1000</strong></li><li><strong>1 &lt;= m + n &lt;= 2000</strong></li><li><strong>-106 &lt;= nums1[i], nums2[i] &lt;= 106</strong></li></ul><p><br>&nbsp;</p><p><strong>Follow up: The overall run time complexity should be O(log (m+n)).</strong></p>
        </div>
      </div>
</div>
</div>
  <div class="relative py-1 sm:max-w-xl sm:mx-auto">
            <prism language="javascript">{{ code }}</prism>
      </div>
</div>
      </div>
    </div>
  </div>
</template>

<script>
import Prism from 'vue-prism-component'

export default {
  data() {
    return {
      code: `const axios = require('axios');

module.exports = async (context) => {
  const answers = await axios.get(
    'https://hire-298805-default-rtdb.firebaseio.com/regexAnswers.json'
  );
  const res = answers.data;
  const submitted = context.params;
  let score = 0
  Object.keys(submitted).forEach((e) => {
    const statement = submitted[e].sentence
    const pattern = new RegExp(submitted[e].selected.toString(),"i")
    if (statement.match(pattern) && statement.match(pattern)[0].toString().trim() == res[e]){
      score += submitted[e].marks
    }
  })
  Object.keys(submitted).forEach((e) => {
    const statement = submitted[e].sentence
    const pattern = new RegExp(submitted[e].selected.toString(),"i")
    if (statement.match(pattern) && statement.match(pattern)[0].toString().trim() == res[e]){
      score += submitted[e].marks
    }
  })
  
  return score
};
`
    }
  },
  components: {
    Prism
  }
}
</script>