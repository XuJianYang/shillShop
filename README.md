"""
设置一个购买技能的系统，该系统具有添加购物车，结算，查询商品列表等功能
    预置技能
    101:{'name':'倚天屠龙剑','price':'100000'}

系统主界面
**********
商店
**********
按1购买
按2结算
按q退出
**********
"""
mainui="""
系统主界面
**********
商店
**********
按1购买
按2结算
按q退出
**********
"""
product_info={
101:{'name':'倚天剑','price':100000},
102:{'name':'屠龙剑','price':100000},
103:{'name':'九阳神功','price':100000},
104:{'name':'九阴白骨爪','price':90000},
105:{'name':'乾坤大挪移','price':88880},
106:{'name':'七伤拳','price':70000}
}

#购物车
cart=[]

#主方法
def main():
    while True:
        print(mainui.strip())
        select=input()   #input（）输入的是字符串
        if select.lower()=='q':
            print('谢谢惠顾，欢迎下次再来')
            break
        elif select=='1':
            paint_Info()
            gotoCart()

        elif select=='2':
            order_print()
            pay(total_price())

        else:
            print('请重新输入')

#打印商品信息
def paint_Info():
    print('*'*50)
    print('编号'.center(10)+'商品名'.center(10)+'价格'.center(10))
    for k in product_info.keys():
        print(str(k).center(10)+product_info[k]['name'].center(10)+str(product_info[k]['price']).center(10))
    print('*' * 50)
#添加商品到购物车
def gotoCart():
    paint_Info()
    while True:
        id=int(input('请输入商品编号：'))
        if id not in product_info:
            print('你选择的商品不存在，请重新输入')
            continue
        num=int(input("请输入商品的数量"))
        #遍历购物车数据,判断是否已购同类型商品
        flag=True
        for c in cart:
            if id in c:      #c为字典类型，看字典里是否有id
                c[id]['count']+=num
                c[id]['price']+=c[id]['price']*num
                flag=False
                jud=1
                break

        if flag==True:
            cart_dict={id:{'name':product_info[id]['name'],\
                        'unitprice':product_info[id]['price'],'count':num,\
                       'price':product_info[id]['price']*num}}
            print(cart_dict)
            cart.append(cart_dict)
        print('添加购物车成功,是否继续(Y/N):')
        s=input()
        if s.upper()=='Y':
            continue
        if s.upper()=='N':
            break

#打印订单
def order_print():
    print('*'*50)
    print('*购物车')
    print('*'*50)
    print('%-8s %-8s %-8s %-8s %-8s '%('编号','商品名','单价','数量','总价'))
    for order in cart:
        for k,v in order.items():
            print('%-8s %-8s %-8s %-8s %-8s ' % (k, v['name'], \
                v['unitprice'], v['count'], v['price']),end='')
        print()
    print('*'*50)
#计算订单总价
def total_price():
    total_price=0
    for order in cart:
        for v in order.values():
            total_price+=v['price']
    return  total_price

#支付方法
def pay(total_price):
    print('本次共计:{}件商品，总金额:为{}元，请输入金额'.format(len(cart),total_price))
    while True:
        money=int(input())
        if money<total_price:
            print('金额不足，请重新输入')
        else:
            print('应收金额:%.2f,实收金额%.2f,找零:%.2f'%(total_price,money,money-total_price))
            cart.clear()
            break


if __name__ == '__main__':
     main()
