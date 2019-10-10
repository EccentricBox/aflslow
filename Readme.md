# 下一个种子输入策略：
1. 优先考虑s(i)最小的路径i的输入
2. 优先考虑f(i)最小的路径i的输入
其中s(i)是路径i的模糊次数，f(i)是能量，在fuzz.c文件的update_bitmap_score函数中修改

# 能量分配：
在fuzz.c文件的calculate_score函数中修改

# fuzzone函数
fuzzone函数中calculate_score函数得到pref_score作为seed的得分，根据pref_score得分进入common_fuzz_stuff函数（
-》save_if_interesting->...->updata_bitmap_score函数）,updata_bitmap_score函数对新路径，对合适的种子进行标记为favorite
在fuzzone函数中，不合适的种子进入abandon_entry,从而不会被fuzz。
