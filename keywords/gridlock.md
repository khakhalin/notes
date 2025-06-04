# Gridlock

Parents: [[system_design]], [[database]], [[devops]]
See also: [[postgres]], [[concurrency]]

#db #concurrency #realtime


Basically, when 2 db processes lock each other, and neither can finish. 🔥

Best ways to fight:
* Introduce time-outs
* Perform locks manually, don't rely on [[sql]] optimization
* Lock things at the last possible moment, to minimize the time when one process blocks another one
* The order in which locks are taken apparently matters. If some of your transactions lock the table first on key A, then on key B, and some other transactions do it in the opposite order (first B, then A), then these two transactions may sometimes gridlock each other. While if you always do first A then B, then it would not happen. 🔥🔥🔥 _I don't understand this. Investigate how locks work? Why exactly would they block each other? Try to understand that and write it more step-by-step here_

# Refs

Nice case-studies for avoiding gridlocks in [[postgres]] (also cited there):
https://www.citusdata.com/blog/2018/02/22/seven-tips-for-dealing-with-postgres-locks/