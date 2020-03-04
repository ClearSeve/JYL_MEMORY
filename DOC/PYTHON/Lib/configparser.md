# configparser

## 创建ini

```
config = configparser.ConfigParser()
config['sec'] = {'KEY1': '0','KEY2': '1'}
config['sec']['KEY3'] = '2'
with open('D:\\1.ini', 'w') as configfile:
    config.write(configfile)
```

## 读写ini

```
config = configparser.ConfigParser()
config.read('D:\\1.ini')
val = config['sec']['KEY3']
print(val)

config['sec']['KEY4'] = '4'
with open('D:\\1.ini', 'w') as configfile:
    config.write(configfile)
```
