import time
import random

# 字典调用方法
def get_key(dictionary, value, default=None):
    return next((k for k, v in dictionary.items() if v == value), default)

def get_value(dictionary, key, default=None):
    return next((v for k, v in dictionary.items() if k == key), default)

# 游戏循环装饰
def wrapper(game):
    def loopfunc(*args):
        loop_arg = True
        while loop_arg:
            game(*args)
            judge = input("输入 'Y' 继续游戏，输入其他任意键结束游戏：")
            if judge.lower() != 'y':
                loop_arg = False
    return loopfunc

# 游戏1：猜拳
@wrapper
def guess_hand(name, hand_dict, score):
    print('游戏开始！')
    person_choice = int(input('\'剪刀\':1, \'石头\':2, \'布\':3\n请输入对应数字：'))
    rand_choice = random.randrange(1, 4, 1)
    computer_choice = get_key(hand_dict, rand_choice)

    print('结果展示！')
    time.sleep(1)
    print(f'计算机的选择：{computer_choice}\n')
    if person_choice == rand_choice:
        print('平局！得分为零！')
    elif (person_choice == 1 and rand_choice == 2) or (person_choice == 2 and rand_choice == 3) or (person_choice == 3 and rand_choice == 1):
        score -= 1
        print('本局落败！得分为-1！')
    else:
        score += 1
        print('本局获胜！得分为1！')

    update_score(name, '猜拳', score)  # 更新分数

# 游戏2：对战
@wrapper
def fight(name, dict_fight, dict_life, score):
    person_life = 1000
    computer_life = 1000
    
    print('游戏开始！')
    print("""招式表：
             1\t2\t3\t4\t5
    出拳\t飞踢\t嘲讽\t究极必杀\t回血
    -200\t-250\t-100\t-390\t190
    双方血量：1000\n""")
    
    while person_life > 0 and computer_life > 0:
        # 用户回合
        person_choice = int(input('输入数字出招：'))
        person_heal = 0  # 用户回血量初始化

        if person_choice == 5:  # 用户回血技能
            person_heal = 190
            print(f'你使用了回血技能，回复了190点血量！')
        
        # 计算机回合
        computer_choice = random.randrange(1, 6, 1)
        computer_heal = 0  # 计算机回血量初始化

        if computer_choice == 5:  # 计算机回血技能
            computer_heal = 190
            print(f'计算机使用了回血技能，回复了190点血量！')
        
        # 计算招式效果
        person_damage, computer_damage = calculate_result(person_choice, computer_choice, dict_life)

        # 更新双方血量
        person_life = max(0, min(1000, person_life + computer_damage))
        computer_life = max(0, min(1000, computer_life + person_damage))

        # 应用回血技能，但不给对方加血
        person_life = max(0, min(1000, person_life + person_heal - max(0, computer_damage)))
        computer_life = max(0, min(1000, computer_life + computer_heal - max(0, person_damage)))

        # 显示回合结果
        print(f'你出招：{get_value(dict_fight, person_choice)}    计算机出招：{get_value(dict_fight, computer_choice)}')
        print(f'计算机的血量:{computer_life}    你的血量:{person_life}')

    if person_life <= 0:
        print('你输了！此局落败！')
        score -= 1
    else:
        print('你赢了！此局获胜！')
        score += 1

    update_score(name, '对战', score)  # 更新分数

# 计算对战结果
def calculate_result(person_choice, computer_choice, dict_life):
    person_damage = dict_life.get(person_choice, 0)
    computer_damage = dict_life.get(computer_choice, 0)

    return person_damage, computer_damage

# 更新分数的函数
def update_score(name, game_type, score):
    file_data = ''
    with open('image.txt', mode='r', encoding='utf-8') as namelist:
        for line in namelist:
            if name in line:
                line = line.replace(f'{game_type}:0', f'{game_type}:{score}')
            file_data += line
    with open('image.txt', mode='w', encoding='utf-8') as namelist:
        namelist.write(file_data)


# 注册操作
def new_one():
    print('请注册！')
    name = input('输入你的名字：')
    password = input('请输入新密码：')
    with open('image.txt', mode='a', encoding='utf-8') as namelist:
        every_line = f'{name}-{password}-猜拳:0-对战:0\n'
        namelist.write(every_line)
    print("正在注册...")
    time.sleep(1.5)
    print(f'注册成功，{name}用户！请牢记密码。')
    return True

# 登录操作
def log_in():
    print('请登录！')
    name = input('输入你的名字：')
    password = input('请输入密码：')
    with open('image.txt', mode='r', encoding='utf-8') as namelist:
        for line in namelist:
            line = line.strip()
            line_list = line.split('-')
            if name == line_list[0] and password == line_list[1]:
                print(f"""---成功登录！---
                      您的得分：
                      猜拳：{line_list[2]}分  对战：{line_list[3]}分
                      ---请选择游戏---""")
                time.sleep(1)
                while True:
                    global score
                    choice = int(input('\n输入数字决定游戏：1...猜拳/2...对战/3...查询分数/4...退出'))
                    time.sleep(1)
                    if choice == 1:
                        hand_dict = {'剪刀': 1, '石头': 2, '布': 3}
                        guess_hand(name, hand_dict, score)
                    elif choice == 2:
                        dict_fight = {1: '出拳！', 2: '飞踢！', 3: '嘲讽！', 4: '究极必杀！', 5: '回血'}
                        dict_life = {1: -150, 2: -200, 3: -100, 4: -390, 5: 190}
                        fight(name, dict_fight, dict_life, score)
                    elif choice == 3:
                        print(f'\n{name}用户的得分：')
                        print(f'猜拳：{line_list[2]}分  对战：{line_list[3]}分')
                    elif choice == 4:
                        time.sleep(1)
                        print('再见！')
                        time.sleep(2.5)
                        break
                    else:
                        print('抱歉，不存在别的游戏！请重选')
            else:
                print('信息有误，请重新输入。')
                time.sleep(0.5)
                log_in()

# 选择模块/注册/登录/退出------------------------------主函数！
print("""
----------------
 欢迎来到新游戏！
           
 输入'1'选择注册
 输入'2'选择登录
 输入'3'选择退出
----------------
""")
score = 0
while True:
    login = int(input('操作：'))
    if login == 1:
        new_one()  # 注册
        log_in()
        break
    elif login == 2:
        log_in()  # 登录
        break
    elif login == 3:
        print('再见！')  # 退出
        break
    else:
        print('没有此选项。请重选')
