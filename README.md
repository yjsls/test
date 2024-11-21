11月2日进度
完成gitee仓库的建立，并上传之前完成的开源代码，同时根据老师提供的指导，接下来进行项目的调整，设计一个反向确认生成的 SQL 模拟数据的数据类型。
11月18日进度
一.在已安装SQLAlchemy和Faker下，Python脚本：
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from faker import Faker

# 创建数据库引擎和基本模型
engine = create_engine('sqlite:///example.db')
Base = declarative_base()

# 定义用户模型
class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String)
    address = Column(String)

# 创建表
Base.metadata.create_all(engine)

# 初始化 Faker
fake = Faker()

# 插入模拟数据
Session = sessionmaker(bind=engine)
session = Session()

for _ in range(100):  # 生成100条数据
    user = User(
        name=fake.name(),
        email=fake.email(),
        address=fake.address()
    )
    session.add(user)

# 提交数据
session.commit()

# 反向获取数据类型
def get_column_types(model):
    column_types = {}
    for column in model.__table__.columns:
        column_types[column.name] = str(column.type)
    return column_types

# 获取用户模型的列类型
user_column_types = get_column_types(User)
print("Column Types:", user_column_types)

# 关闭会话
session.close()
二.代码说明
创建数据库和表：使用 SQLAlchemy 创建一个 SQLite 数据库和一个用户表。
生成模拟数据：使用 Faker 生成 100 条用户数据并插入数据库。
反向获取数据类型：定义 get_column_types 函数，提取模型中每一列的数据类型并以字典形式返回。
输出结果：打印出每一列的名称及其数据类型。
三.运行脚本
运行该脚本后，你将看到类似以下的输出，展示每一列的名称和对应的数据类型：
Column Types: {'id': 'INTEGER', 'name': 'VARCHAR', 'email': 'VARCHAR', 'address': 'VARCHAR'}
