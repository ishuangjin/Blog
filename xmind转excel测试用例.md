## 需求

因个人习惯使用xmind编写测试用例，从xmind中导出到excel还需要进行排版，所以使用python简单写了个读取xmind内容转化为excel的工具

#### xmind格式

最后一级目录为用例详情，需要在用例详情中加上`前置：` `步骤：` `结果：` 其中之一作为起始，二级目录至用例详情之前由`_`连接组成用例名称

![xmind转excel测试用例-2022-08-03-14-50-24](http://cache.ishuangjin.cn/img/xmind转excel测试用例-2022-08-03-14-50-24.png)

#### 生成的excel

![xmind转excel测试用例-2022-08-03-14-51-24](http://cache.ishuangjin.cn/img/xmind转excel测试用例-2022-08-03-14-51-24.png)

#### 代码实现

通过xmindparser模块读取xmind数据，pandas写入excel文件

```python
from xmindparser import xmind_to_dict
import pandas as pd


def get_data(xm):  # 提取所有用例数据
    case_name = []
    case_list = []
    case_step = []
    case_result = []
    case_front = []

    def get_case_name(xm):
        global get_case_name_for
        for i in range(len(xm)):
            get_case_name_for = True
            if xm[i].__contains__('topics'):  # 带topics标签意味着有子标题，递归执行方法
                case_name.append(xm[i]['title'])
                get_case_name(xm[i]['topics'])
            else:  # 不带topics意味着无子标题，此级别既是用例详情
                if xm[i]['title'][:3] == "前置：":
                    case_front.append(xm[i]['title'][3:])
                elif xm[i]['title'][:3] == "步骤：":
                    case_step.append(xm[i]['title'][3:])
                elif xm[i]['title'][:3] == "结果：":
                    case_result.append(xm[i]['title'][3:])

        case_name_str = "_".join(case_name)  # 转化成字符串
        if get_case_name_for:
            case_list.append(case_name_str)
            if len(xm) == 2:  # 如果只有步骤和结果则给前置填充'/'
                case_front.append("/")
        try:
            case_name.pop()
            get_case_name_for = False
        except IndexError:
            pass

    # xm = xmind_to_dict("项目名称.xmind")[0]['topic']['topics']  # 读取xmind文件
    get_case_name(xm)
    case_count = len(case_list)
    # print(json.dumps(xm, indent=2, ensure_ascii=False))
    return case_count, case_list, case_front, case_step, case_result


def save_data(xm, file_name="path_to_file.xlsx", case_index="", demand_id="请填写需求ID",
              case_type="功能测试", case_state="正常", case_level="中", case_creater="tester"):
    row0 = ["用例目录", "用例名称", "需求ID", "前置条件", "用例步骤", "预期结果", "用例类型", "用例状态", "用例等级",
            "创建人", "回归状态", "回归时间", "执行人", "执行时间", "备注", "实际结果"]  # 定义表头
    some_cos = get_data(xm)

    def lsbd(field):  # list_build
        return [field for _ in range(some_cos[0])]

    data_value_list = [lsbd(case_index), some_cos[1], lsbd(demand_id), some_cos[2], some_cos[3], some_cos[4],
                       lsbd(case_type), lsbd(case_state), lsbd(case_level), lsbd(case_creater),
                       lsbd(""), lsbd(""), lsbd(""), lsbd(""), lsbd(""), lsbd("")]

    def get_data_value(data_value_list):
        case_data = {}
        n = -1
        for data_title in row0:
            n += 1
            case_data[data_title] = data_value_list[n]
        return case_data

    case_data = get_data_value(data_value_list)
    df = pd.DataFrame(case_data, columns=row0)
    # print(df)
    print(df.loc[:, "用例名称"], "\n----------------------\n")
    df.to_excel(file_name, index=None, sheet_name="Sheet1")


def main():
    xmind_file = "test.xmind"  # 要操作的文件
    file_name = xmind_file.replace(".xmind", ".xlsx")
    case_index = "test-2022年6月15日需求"  # case存放路径
    demand_id = "1024497"  # 需求ID

    xm = xmind_to_dict(xmind_file)[0]['topic']['topics']  # 读取xmind文件
    save_data(xm, file_name, case_index, demand_id)


if __name__ == '__main__':
    try:
        main()
    except PermissionError:
        print("Error：文件被占用，请关闭已打开的xlsx文件")
    else:
        print("Success")

```
