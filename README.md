from pygame import * 
from random import randint
from time import sleep
win_width=500
win_height=500
window = display.set_mode((win_width,win_height))#создаем объект окна игры - и задаем ему размер
display.set_caption("MY GAME")
WHITE=(255,255,255)#Создаем белый цветс помощью RGB
RED=(255,0,0)
BLUE=(0,0,255)
#Создаем персонажа
hero = image.load('platform.png')                  #Surface([70,30])#поверхность 50 на 50 пикселей
hero = transform.scale(hero,(70,30))
rect1 = hero.get_rect()#создаем хитбокс
rect1.x=230
rect1.y=450
enemy = image.load('bird.png')                                                     #Surface([20,20])
enemy = transform.scale(enemy,(20,20))
rect2 = enemy.get_rect()
rect2.x=50
rect2.y=50
cathced = 0
passed = 0
FPS = 150
font.init()
font24=font.SysFont('Arial', 24)
text1 = font24.render('Поймано:'+ str(cathced),True,(255,0,0))
text2 = font24.render('Пропущено:'+ str(passed),True,(255,0,0))
text5 = font24.render('FPS'+ str(FPS),True,(255,0,0))
textWIN = font24.render('Победа!',True,(255,0,0))
textLOSE = font24.render('Проигрыш!',True,(255,0,0))
background = image.load('fon.png')
background = transform.scale(background,(win_width,win_height))
clock = time.Clock()
game='in_process'
while True:
	if game =='in_process':
		window.blit(background,(0,0))
		window.blit(text1,(0,20))
		window.blit(text5,(400,20))
		window.blit(text2,(0,50))
		window.blit(enemy,(rect2.x,rect2.y))
		window.blit(hero,(rect1.x,rect1.y))#Располагает объект в указанных координатах
		display.update()#фиксируем изменения - пишется всегда в конце
		rect2.y+=1
		if rect2.y > win_height:
			passed += 1
			text2 = font24.render('Пропущено:'+ str(passed),True,(255,0,0))
			rect2.x = randint(0,win_width) - 20
			rect2.y = 0
		if rect1.colliderect(rect2):
			FPS += 20
			cathced += 1
			text1 = font24.render('Поймано:'+ str(cathced),True,(255,0,0))
			rect2.x = randint(0,win_width) - 20
			rect2.y = 0
		text5 = font24.render('FPS'+ str(FPS),True,(255,0,0))
		#Управление для персонажа
		keys = key.get_pressed()
		if keys[K_LEFT] and rect1.x>0:
			rect1.x-=1
		elif keys[K_RIGHT] and rect1.x+70<win_width:
			rect1.x+=1
		if passed >= 5:
			game = 'lose'
		if cathced >= 10:
			game = 'win'
	elif game == 'lose':
		window.blit(textLOSE,(250,250))
		display.update()
	elif game == 'win':
		window.blit(textWIN,(250,250))
		display.update()
	for i in event.get():		
		if i.type==QUIT:
			quit()
		if i.type == KEYDOWN and game != 'in_process':
			if i.key == K_SPACE:
				cathced = 0
				passed = 0
				FPS = 150
				textWIN = font24.render('Победа!',True,(255,0,0))
				display.update()
				textLOSE = font24.render('Проигрыш!',True,(255,0,0))
				display.update()
				text5 = font24.render('FPS'+ str(FPS),True,(255,0,0))
				display.update()
				rect2.x = randint(0,win_width) - 20
				rect2.y = 0
				rect1.x=230
				game = 'in_process'
	clock.tick(FPS)
****
