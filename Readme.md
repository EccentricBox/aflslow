## 下一个种子输入策略：
1. 优先考虑s(i)最小的路径i的输入
2. 优先考虑f(i)最小的路径i的输入
其中s(i)是路径i的模糊次数，f(i)是能量，在fuzz.c文件的update_bitmap_score函数中修改

## 能量分配：
在fuzz.c文件的calculate_score函数中修改

## 主流程
1. afl_fuzz的main函数会解析用户输入命令，检查环境变量的设置、输入输出路径、目标文件。程序定义了结构体queue_entry链表维护fuzz中使用的文件。
2. 函数perform_dry_run() 会使用初始的测试用例进行测试，确保目标程序能够正常执行,生成初始化的queue和bitmap。
3. 函数 cull_queue() 会对初始队列进行筛选（更新favored entry）。遍历top_rated[]中的queue，然后提取出发现新edge的entry，并标记为favored，使得在下次遍历queue时，这些entry能获得更多执行fuzz的机会。
4. 进入while(1)开始fuzz循环
- 进入循环后第一部还是 cull_queue() 对queue进行筛选
- 判断queue_cur是否为空，如果是，则表示已经完成对队列的遍历，初始化相关参数，重新开始遍历队列
- fuzz_one() 函数会对queue_cur所对应文件进行fuzz，包括(跳过-calibrate_case-修剪测试用例-对用例分配能量-变异)
- 判断是否结束，更新queue_cur和current_entry
- 当队列中的所有文件都经过变异测试了，则完成一次”cycle done”。整个队列又会从第一个文件开始，再次继续进行变异


### fuzzone函数
fuzzone函数中calculate_score函数得到pref_score作为seed的得分，根据pref_score得分进入common_fuzz_stuff函数（
-》save_if_interesting->...->updata_bitmap_score函数）,updata_bitmap_score函数对新路径，对合适的种子进行标记为favorite
