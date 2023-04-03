# enenen.github.io
weneneCSDN博客




(1).利用读取xml的方法读取单链表中
import xml.etree.cElementTree as ET
import link


"字典转换成列表函数"
def dict_to_list(temp_dict):
    temp_list = []
    "遍历字典key，val的值，并以列表返回可遍历的值的元组数组"
    for key, val in temp_dict.items():
        temp_list.append([key, val])
    "返回列表"
    return temp_list


"获取父节点"
def get_all(father):
    """xml文件数据返回结构"""
    xml_strcture = []
    for child in father:
        attrib_list = dict_to_list(child.attrib)
        tag_name = child.tag
        xml_strcture.append([tag_name, attrib_list])

        children = get_all(child)
        if len(children) != 0:
            for cur in children:
                xml_strcture.append(cur)

    return xml_strcture


def get_xml_content(path):
    tree = ET.parse(path)
    root = tree.getroot()
    xml_structure = get_all(root)
    return xml_structure
    # tag = root.tag()
    # tag_name = list
    # attrib_list = list
    # temp = list
    # for tag in root:
    #     tag_name.append(list(tag.tag))
    #
    # return


if __name__ == '__main__':
    temp = get_xml_content("home.xml")
    print(temp)
    temp2 = []
    new_SingleLinkList = link.SingleLinkList()
    for xml_tag in temp:
        print(xml_tag)
        new_SingleLinkList.append(xml_tag)

    length = new_SingleLinkList.length()
    print(length)
    for item in new_SingleLinkList.travel():
        print(item)

注意最后一行必须空出来否则会发生报错
定义时注意返回值的定义none若多重定义会发生报错




（2）单链表的代码
class Node(object):
    """定义一个节点类"""

    def __init__(self, item):
        self.val = item
        self.next = None


class SingleLinkList(object):
    """定义一个单链表"""

    def __init__(self):
        self.head = None

    def is_empty(self):
        """判断链表是否为空"""
        return self.head is None

    def length(self):
        """返回链表长度"""
        cur = self.head
        count = 0
        while cur is not None:
            count += 1
            cur = cur.next
        return count

    def travel(self):
        """遍历链表"""
        cur = self.head
        while cur is not None:
            print(cur.val, end=' ')
            yield cur.val    //yield的使用：如果条件成立则直接退出循环
            cur = cur.next

    def add(self, item):
        """在链表头添加元素"""
        node = Node(item)
        node.next = self.head
        self.head = node

    def append(self, item):
        """在链表尾添加元素"""
        node = Node(item)
        if self.is_empty():
            self.head = node
        else:
            cur = self.head
            while cur.next is not None:
                cur = cur.next
            cur.next = node

    def insert(self, pos, item):
        """在指定位置pos插入元素"""
        node = Node(item)
        if pos <= 0:
            self.add(item)
        elif pos > self.length() - 1:
            self.append(item)
        else:
            cur = self.head
            count = 0
            while count < pos - 1:
                cur = cur.next
                count += 1
            node.next = cur.next
            cur.next = node

    def remove(self, item):
        """删除节点"""
        cur = self.head
        if cur.val == item:
            self.head = cur.next
        else:
            while cur.next:
                if cur.next.val == item:
                    cur.next = cur.next.next
                    break
                else:
                    cur = cur.next

    def search(self, item):
        """查找节点是否存在"""
        cur = self.head
        while cur is not None:
            if cur.val == item:
                return True
            else:
                cur = cur.next
        return False

    def find(self, item):
        """查找数据所在的位置"""
        cur = self.head
        count = 0
        while cur.val is not None:
            if cur.val == item:
                return count
            else:
                count += 1   //利用计数器返回数据所在的位置
                cur = cur.next
        return count


if __name__ == '__main__':
    L = SingleLinkList()
    print(L.is_empty())
    print(L.length())

    L.append(3)
    L.add(999)
    L.insert(-3, 110)
    L.insert(99, 111)
    print(L.is_empty())
    print(L.length())
    L.travel()
    L.remove(111)
    L.travel()
    print(L.search(999))
    L.append(7)
    L.travel()
    L.find(7)
    print(L.find(7))
