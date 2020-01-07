# TP_LifeiZhu
#################################################
# Project Name: Hungry Panda
#
# Your name:Lifei Zhu
# Your andrew id:lifeiz
# Version 1

#################################################


from cmu_112_graphics import *
from tkinter import *
from PIL import Image
import random


class Panda(object):
    def __init__(self,sprite,x,y):
        self.frame = 0
        self.sprite = sprite
        self.x = x*x /////////////////////////////
        self.y = y
        self.pandaWidth = 64
        self.pandaHeight = 90
    def panda_timerFired(self):
        self.frame += 1
        self.frame %= len(self.sprite)
    def panda_redrawAll(self,canvas,cx,cy):
        frameNew = self.sprite[self.frame]
        canvas.create_image(cx, cy, image=ImageTk.PhotoImage(frameNew))


class SplashScreenMode(Mode):
    def appStarted(mode):
        urlStart = 'https://i.loli.net/2019/11/21/j5ty7kFMuGo4iUH.png'
        spriteStart = mode.loadImage(urlStart)
        spriteOutStart = mode.scaleImage(spriteStart,1.3)
        mode.screenStart = spriteOutStart

    def mousePressed(mode,event):
        mode.cursor = (event.x,event.y)
        mode.cursorImage = mode.closedBox


    def redrawAll(mode, canvas):
        canvas.create_image(mode.width//2,mode.height//2,
                            image=ImageTk.PhotoImage(mode.screenStart))
        font = 'Arial 26 bold'
        canvas.create_text(mode.width/2, 50, text='Welcome to Hungry Panda!', font=font)
        canvas.create_text(mode.width/2, 150,
        text='Help the panda to win the bamboo', font=font)
        canvas.create_text(mode.width/2, 250,
        text='THREE steps to win', font=font)
        canvas.create_text(mode.width/2, 350,
        text='Click START to take this adventure', font=font)
        canvas.create_text(mode.width/2, 450, text='Click HELP for help!', font=font)
    def keyPressed(mode, event):
        mode.app.setActiveMode(mode.app.gameModeOne)

class GameModeOne(Mode):
    def appStarted(mode):
        url = 'https://i.loli.net/2019/11/21/xHRZrd4vCLYoDz3.png'
        spritestrip0 = mode.loadImage(url)
        spritestrip = mode.scaleImage(spritestrip0, 1)
        mode.spritesRight = []
        for i in range(16):
            sprite = spritestrip.crop((64*i, 0, 64*(i+1), 90))
            mode.spritesRight.append(sprite)
        mode.pandaX = 64 # panda's center
        mode.pandaY = mode.height//3-45 # panda's center
        mode.panda = Panda(mode.spritesRight,mode.pandaX,mode.pandaY)
        urlOpenBox = 'https://i.loli.net/2019/11/21/gbLYq9RzKBI3trV.png'
        spritestripOpenBox0 = mode.loadImage(urlOpenBox)
        spritestripOpenBox = mode.scaleImage(spritestripOpenBox0, 1/8)
        mode.cursorImage = spritestripOpenBox
        mode.cursor = (0,0)


    def keyPressed(mode, event):
        if (event.key == "h"):
            mode.app.setActiveMode(mode.app.helpModeOne)
        elif (event.key == "a"):
            mode.app.setActiveMode(mode.app.gameModeTwo)

    def timerFired(mode):
        mode.panda.panda_timerFired()

    def mouseMoved(mode,event):
        mode.cursor = (event.x,event.y)

    def redrawAll(mode, canvas):
        canvas.create_rectangle(0, 0, mode.width, mode.height)
        cx, cy = mode.panda.x, mode.panda.y
        mode.panda.panda_redrawAll(canvas,cx,cy)
        canvas.create_rectangle(0, mode.height//3, mode.width//5, mode.height,
                                fill='brown')
        canvas.create_rectangle(4*mode.width//5, mode.height//3,
                                mode.width,mode.height,fill='brown')
        canvas.create_rectangle(mode.width//5, 2*mode.height//3,
                                4*mode.width//5,mode.height,fill='light blue')
        curX,curY = mode.cursor
        canvas.create_image(curX,2*mode.height//3,
                            image=ImageTk.PhotoImage(mode.cursorImage))

class GameModeTwo(Mode):
    def appStarted(mode):
        url = 'https://i.loli.net/2019/11/21/xHRZrd4vCLYoDz3.png'
        spritestrip0 = mode.loadImage(url)
        spritestrip = mode.scaleImage(spritestrip0, 1)
        mode.spritesRight = []
        for i in range(16):
            sprite = spritestrip.crop((64*i, 0, 64*(i+1), 90))
            mode.spritesRight.append(sprite)
        mode.pandaX = 64 # panda's center
        mode.pandaY = mode.height//3-45 # panda's center
        mode.panda = Panda(mode.spritesRight,mode.pandaX,mode.pandaY)

    def movePanda(mode, dx, dy):
        mode.panda.x += dx
        mode.panda.y += dy

    def timerFired(mode):
        mode.panda.panda_timerFired()

    def mouseMoved(mode,event):
        mode.cursor = (event.x,event.y)

    def keyPressed(mode, event):
        if (event.key == "h"):
            mode.app.setActiveMode(mode.app.helpModeTwo)
        elif (event.key == "s"):
            mode.app.setActiveMode(mode.app.gameModeThree)
        elif (event.key == "Left"):    
            # mode.panda.sprite = mode.spritesLeft
            mode.movePanda(-5, 0)
        elif (event.key == "Right"):
            # mode.panda.sprite = mode.spritesRight
            mode.movePanda(+5, 0)

    def redrawAll(mode, canvas):
        canvas.create_rectangle(0, 0, mode.width, mode.height,fill='grey')
        cx, cy = mode.panda.x, mode.panda.y
        mode.panda.panda_redrawAll(canvas,cx,cy)

class GameModeThree(Mode):
    def appStarted(mode):
        url = 'https://i.loli.net/2019/11/21/xHRZrd4vCLYoDz3.png'
        spritestrip0 = mode.loadImage(url)
        spritestrip = mode.scaleImage(spritestrip0, 1)
        mode.spritesRight = []
        for i in range(16):
            sprite = spritestrip.crop((64*i, 0, 64*(i+1), 90))
            mode.spritesRight.append(sprite)
        mode.pandaX = 64 # panda's center
        mode.pandaY = mode.height//3-45 # panda's center
        mode.panda = Panda(mode.spritesRight,mode.pandaX,mode.pandaY)

        urlClosedBox = 'https://i.loli.net/2019/11/21/gH2Q6fNZbMjeqCv.png'
        spritestripClosedBox0 = mode.loadImage(urlClosedBox)
        spritestripClosedBox = mode.scaleImage(spritestripClosedBox0, 1/10)
        mode.bambooImage = spritestripClosedBox
        mode.bamboosCenter = [(random.randrange(mode.width//3,mode.width),
                      random.randrange(60, mode.height)) for _ in range(10)]



    def keyPressed(mode, event):
        if (event.key == "h"):
            mode.app.setActiveMode(mode.app.helpModeThree)

    def redrawAll(mode, canvas):
        canvas.create_rectangle(0, 0, mode.width, mode.height,fill='pink')
        cx, cy = mode.panda.x, mode.panda.y
        mode.panda.panda_redrawAll(canvas,cx,cy)
        for cx,cy in mode.bamboosCenter:
            canvas.create_image(cx,cy,
                            image=ImageTk.PhotoImage(mode.bambooImage))




class GameOverMode(Mode):
    def redrawAll(mode, canvas):
        font = 'Arial 26 bold'
        canvas.create_text(mode.width/2, 150, text='You lose!', font=font)
        canvas.create_text(mode.width/2, 350, text='Press r to return to the game!', font=font)

    def keyPressed(mode, event):
        if event.key == "r":
            mode.app.gameModeOne.appStarted()
            mode.app.setActiveMode(mode.app.gameModeOne)

class GameWinMode(Mode):
    def redrawAll(mode, canvas):
        font = 'Arial 26 bold'
        canvas.create_text(mode.width/2, 150, text='You win!', font=font)
        canvas.create_text(mode.width/2, 350, text='Press r to return to the game!', font=font)

    def keyPressed(mode, event):
        if event.key == "r":
            mode.app.setActiveMode(mode.app.gameModeOne)

class HelpModeOne(Mode):
    def redrawAll(mode, canvas):
        font = 'Arial 26 bold'
        canvas.create_text(mode.width/2, 150,
        text='To finish step1, you need to score 10', font=font)
        canvas.create_text(mode.width/2, 250,
        text='Arrows to move, \nand mouse to move and click', font=font)
        canvas.create_text(mode.width/2, 350, text='Press s for superHelp', font=font)

    def keyPressed(mode, event):
        mode.app.setActiveMode(mode.app.gameModeOne)

class HelpModeTwo(Mode):
    def redrawAll(mode, canvas):
        font = 'Arial 26 bold'
        canvas.create_text(mode.width/2, 150,
        text='To finish step2, you need to score 10', font=font)
        canvas.create_text(mode.width/2, 250,
        text='Arrows to move, \nand mouse to move and click', font=font)
        canvas.create_text(mode.width/2, 350, text='Press s for superHelp', font=font)

    def keyPressed(mode, event):
        mode.app.setActiveMode(mode.app.gameModeTwo)
class HelpModeThree(Mode):
    def redrawAll(mode, canvas):
        font = 'Arial 26 bold'
        canvas.create_text(mode.width/2, 150,
        text='To finish step3, you need to score 10', font=font)
        canvas.create_text(mode.width/2, 250,
        text='Arrows to move, \nand mouse to move and click', font=font)
        canvas.create_text(mode.width/2, 350, text='Press s for superHelp', font=font)

    def keyPressed(mode, event):
        mode.app.setActiveMode(mode.app.gameModeTwo)

class MyModalApp(ModalApp):
    def appStarted(app):
        app.splashScreenMode = SplashScreenMode()
        app.gameModeOne = GameModeOne()
        app.gameModeTwo = GameModeTwo()
        app.gameModeThree = GameModeThree()
        app.helpModeOne = HelpModeOne()
        app.helpModeTwo = HelpModeTwo()
        app.helpModeThree = HelpModeThree()
        app.gameOverMode = GameOverMode()
        app.gameWinMode = GameWinMode()
        app.setActiveMode(app.splashScreenMode)
        app.timerDelay = 50

app = MyModalApp(width=500, height=500)
