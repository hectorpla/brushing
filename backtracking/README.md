# Backtracking

### 494 Target Sum

original idea: divide into sub tasks: `find(start, end, target)`

* not only: there are n^2 subtasks, and each one takes 1000 iteration O\(n^2\*m\), m is the ranges of sums
* but also: space is great, data structure is very complicated, 3-dimension
* should convert 3d to 2d \(convert range into start/end\)



Optimization:

traverse order: 0 -&gt; end then end -&gt; 0, on the way back, we could store the computed results

