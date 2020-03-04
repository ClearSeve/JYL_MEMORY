# ctime

std::clock_t timeStart = std::clock();//记录当前时间

double t = double(std::clock() - timeStart)/CLOCKS_PER_SEC;//计算程序运行到此处运行的时间，单位是秒
