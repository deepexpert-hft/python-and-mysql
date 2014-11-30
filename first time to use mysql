#__author__ = 'hefangteng'
# coding:utf-8


import urllib
import urllib2
import requests
import re
import MySQLdb

#用requests的方法获取html
def GetHtml(url,data):
    html = requests.post(url,data)
    return html

#获取提交表单得到的value值
def Get_Value(html):
    req = '<tr height=.*?value=\'(.*?)\' />'
    valuelist = re.compile(req).findall(html)
    for value in valuelist:
        return value

#获取所有5位字母的字符串
def Get_all_str():
    a = []
    for i in range(97,123):
        for k in range(97,123):
            for y in range(97,123):
                for x in range(97,123):
                    for m in range(97,123):
                        a.append(chr(i)+chr(k)+chr(y)+chr(x)+chr(m))
    return a

#筛选所有可用的域名
def Do_screen(url):
    a = Get_all_str()
    print '*** Connecting to database'
    try:
    #建立与数据库的连接
        cxn = MySQLdb.connect(host='localhost', user='root', passwd='root', db='python')
    except:
        print "Could not connect to MySQL server."
        exit( 0 )

    cur=cxn.cursor()         # 使用cursor()方法获取操作游标
    print '*** Creating results table'
    cur.execute('CREATE TABLE results(site VARCHAR(8),suffix VARCHAR(8))')     #创建一个results数据库表

    for x in a:
        suffix = '.com'
        data = {'d_name':x+suffix,'dtype':'common'}
        html = GetHtml(url,data)
        value = Get_Value(html.content)
        print x,value
        if(value == 'no'):              #value为no即为可用域名
            values = [x,suffix]
            cur.execute("INSERT INTO results VALUES(%s, %s)" ,values)          #向数据库传入需要的值
            cxn.commit()                #向数据库提交数据
            print '可用域名:'+x+'.com'
    cxn.close()                         # 关闭数据库连接

url = 'http://www.zgsj.com/domain_reg/domaintrans.asp'
Do_screen(url)
