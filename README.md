# Enchancing-Photo-Editing

from PyQt5 import QtCore, QtGui, QtWidgets
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import *
from PIL import Image, ImageFilter, ImageOps, ImageFont, ImageDraw, ImageEnhance
from PIL.ImageFilter import *
import os

class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(1680, 1050)
        MainWindow.setMouseTracking(True)
        MainWindow.setTabletTracking(False)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.label = QtWidgets.QLabel(self.centralwidget)
        self.label.setGeometry(QtCore.QRect(340, 10, 1031, 650))
        self.label.setFrameShape(QtWidgets.QFrame.WinPanel)
        self.label.setFrameShadow(QtWidgets.QFrame.Plain)
        self.label.setText("")
        self.label.setPixmap(QtGui.QPixmap(""))
        self.label.setScaledContents(True)
        self.label.setAlignment(QtCore.Qt.AlignCenter)
        self.label.setObjectName("label")
        self.gridLayoutWidget = QtWidgets.QWidget(self.centralwidget)
        self.gridLayoutWidget.setGeometry(QtCore.QRect(60, 30, 160, 271))
        self.gridLayoutWidget.setObjectName("gridLayoutWidget")
        self.gridLayout = QtWidgets.QGridLayout(self.gridLayoutWidget)
        self.gridLayout.setContentsMargins(0, 0, 0, 0)
        self.gridLayout.setObjectName("gridLayout")
        self.bw = QtWidgets.QPushButton(self.gridLayoutWidget)
        self.bw.setObjectName("bw")
        self.gridLayout.addWidget(self.bw, 0, 0, 1, 1)
        self.filter = QtWidgets.QPushButton(self.gridLayoutWidget)
        self.filter.setObjectName("filter")
        self.gridLayout.addWidget(self.filter, 1, 0, 1, 1)
        self.rotate = QtWidgets.QPushButton(self.gridLayoutWidget)
        self.rotate.setObjectName("rotate")
        self.gridLayout.addWidget(self.rotate, 2, 0, 1, 1)
        self.back = QtWidgets.QPushButton(self.gridLayoutWidget)
        self.back.setObjectName("back")
        self.gridLayout.addWidget(self.back, 4, 0, 1, 1)
        self.front = QtWidgets.QPushButton(self.gridLayoutWidget)
        self.front.setObjectName("front")
        self.gridLayout.addWidget(self.front, 3, 0, 1, 1)
        self.gridLayoutWidget_2 = QtWidgets.QWidget(self.centralwidget)
        self.gridLayoutWidget_2.setGeometry(QtCore.QRect(1460, 30, 160, 281))
        self.gridLayoutWidget_2.setObjectName("gridLayoutWidget_2")
        self.gridLayout_2 = QtWidgets.QGridLayout(self.gridLayoutWidget_2)
        self.gridLayout_2.setContentsMargins(0, 0, 0, 0)
        self.gridLayout_2.setObjectName("gridLayout_2")
        self.blur = QtWidgets.QPushButton(self.gridLayoutWidget_2)
        self.blur.setObjectName("blur")
        self.gridLayout_2.addWidget(self.blur, 1, 0, 1, 1)
        self.tp = QtWidgets.QPushButton(self.gridLayoutWidget_2)
        self.tp.setObjectName("tp")
        self.gridLayout_2.addWidget(self.tp, 3, 0, 1, 1)
        self.frame = QtWidgets.QPushButton(self.gridLayoutWidget_2)
        self.frame.setObjectName("frame")
        self.gridLayout_2.addWidget(self.frame, 2, 0, 1, 1)
        self.brt = QtWidgets.QPushButton(self.gridLayoutWidget_2)
        self.brt.setObjectName("brt")
        self.gridLayout_2.addWidget(self.brt ,4, 0, 1, 1)
        self.gridLayoutWidget_3 = QWidget(self.centralwidget)
        self.gridLayoutWidget_3.setObjectName(u"gridLayoutWidget_3")
        self.gridLayoutWidget_3.setGeometry(QtCore.QRect(570, 690, 471, 161))
        self.gridLayout_3 = QGridLayout(self.gridLayoutWidget_3)
        self.gridLayout_3.setObjectName("gridLayout_3")
        self.gridLayout_3.setSizeConstraint(QLayout.SetDefaultConstraint)
        self.gridLayout_3.setContentsMargins(0, 0, 0, 0)
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 1680, 18))
        self.menubar.setObjectName("menubar")
        self.menuFile = QtWidgets.QMenu(self.menubar)
        self.menuFile.setObjectName("menuFile")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)
        self.actionOpen = QtWidgets.QAction(MainWindow)
        self.actionOpen.setObjectName("actionOpen")
        self.actionSave = QtWidgets.QAction(MainWindow)
        self.actionSave.setObjectName("actionSave")
        self.actionSave_as = QtWidgets.QAction(MainWindow)
        self.actionSave_as.setObjectName("actionSave_as")
        self.menuFile.addAction(self.actionOpen)
        self.menuFile.addAction(self.actionSave)
        self.menubar.addAction(self.menuFile.menuAction())
        #uploading image
        self.actionOpen.triggered.connect(self.uploadfile)
        #save image
        self.actionSave.triggered.connect(self.saveimg)
        #button fun
        self.filter.clicked.connect(self.listfilters)
        self.rotate.clicked.connect(self.rotatefun)
        self.blur.clicked.connect(self.blurfun)
        self.bw.clicked.connect(self.bwfun)
        self.tp.clicked.connect(self.tpfun)
        self.frame.clicked.connect(self.framefun)
        self.back.clicked.connect(self.backfun)
        self.front.clicked.connect(self.frontfun)
        self.brt.clicked.connect(self.brtfun)


        self.retranslateUi(MainWindow)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)
    def showfun(self):
        self.width, self.height = self.img.size
        ratio = self.height / self.width
        #self.label.setMaximumWidth(self.width)
        #self.label.setMaximumHeight(self.height)
        #self.label.setPixmap((self.pixmap))
        new_width=self.width
        new_height=self.height
        if self.height > 1031 or self.width > 650:
            if ratio < 1:
                new_width = 650
                new_height = int(new_width * ratio)
            else:
                new_height = 1031
                new_width = int(new_height * (self.width / self.height))
        self.img = self.img.resize((new_width, new_height))
        if("png" in self.filename[0]):
            img1=self.img.convert('RGB')
            if(self.count==0):
                img1.save("temp3.jpg")
            else:
                img1.save(self.txt+str(self.count)+".jpg")
        else:
            if(self.count==0):
                self.img.save("temp3.jpg")
            else:
                self.img.save(self.txt+str(self.count)+".jpg")




    #back
    def backfun(self):
        self.delete()
        if(self.count==1):
            self.count-=1
            self.label.setPixmap((self.pixmap))
            self.img=Image.open(self.filename[0])
        elif(os.path.exists(self.txt+str(self.count-1)+".jpg")):
            self.count-=1
            txt1=self.txt+str(self.count)+".jpg"
            self.imglist.pop()
            self.imglist.append(txt1)
            pixmap = QPixmap(txt1)
            self.label.setPixmap((pixmap))
            self.img=Image.open(txt1)
    #front
    def frontfun(self):
        self.delete()
        if(os.path.exists(self.txt+str(self.count+1)+".jpg")):
            self.count+=1
            txt1=self.txt+str(self.count)+".jpg"
            self.imglist.pop()
            self.imglist.append(txt1)
            pixmap = QPixmap(txt1)
            self.label.setPixmap((pixmap))
            self.img=Image.open(txt1)
    #upload
    def uploadfile(self):
        self.filename= QFileDialog.getOpenFileName()
        #self.pixmap = QPixmap(self.filename[0])
        self.imglist=self.filename[0].split('/')
        self.img=Image.open(self.filename[0])
        self.txt="img"
        self.count=0
        self.max=0
        self.showfun()
        #self.width, self.height = self.img.size
        #self.label.setMaximumWidth(self.width)
        #self.label.setMaximumHeight(self.height)
        self.pixmap = QPixmap("temp3.jpg")
        self.label.setPixmap((self.pixmap))
        self.img=Image.open("temp3.jpg")
    #save
    def saveimg(self):
        filePath, _ = QFileDialog.getSaveFileName()
        self.img=Image.open(self.txt+str(self.count)+".jpg")
        if filePath=="":
            return 
        self.img.save(filePath)
        for i in range(1,self.max+1):
            os.remove(self.txt+str(i)+".jpg")
        os.remove("temp3.jpg")
        QCoreApplication.instance().quit()
    def delete(self):
        k=0
        try:
            for i in range(self.gridLayout_3.rowCount()):
                for j in range(self.gridLayout_3.columnCount()):
                    self.gridLayout_3.itemAt(k).widget().deleteLater()
                    k+=1
        except:
            pass
    #rotate
    def rotatefun(self):
        self.delete()
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1=self.img.rotate(90,Image.NEAREST,expand=1)
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.img=Image.open(txt1)
        self.showfun()
        self.img=Image.open(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap)) 
    #blur
    def blurfun(self):
        self.delete()
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        nblur=QtWidgets.QPushButton(self.gridLayoutWidget_3)
        nblur.setObjectName("nblur")
        self.gridLayout_3.addWidget(nblur,0,0,1,1)
        nblur.setText("normal blur")
        nblur.clicked.connect(self.nblurtoll)
        bblur=QtWidgets.QPushButton(self.gridLayoutWidget_3)
        bblur.setObjectName("bblur")
        self.gridLayout_3.addWidget(bblur,0,1,1,1)
        bblur.setText("box blur")
        bblur.clicked.connect(self.bblurtoll)
        gblur=QtWidgets.QPushButton(self.gridLayoutWidget_3)
        gblur.setObjectName("gblur")
        self.gridLayout_3.addWidget(gblur,0,2,1,1)
        gblur.setText("gaussian blur")
        gblur.clicked.connect(self.gblurtoll)
    #box blur
    def valuechange1(self,value):
        img1=self.img.filter(BoxBlur(value))
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
    def bblurtoll(self):
        horizontalSlider = QtWidgets.QSlider(self.centralwidget)
        horizontalSlider.setOrientation(QtCore.Qt.Horizontal)
        horizontalSlider.setObjectName("horizontalSlider")
        self.gridLayout_3.addWidget(horizontalSlider, 2, 2, 1, 1)
        img1=self.img.filter(BoxBlur(2))
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
        horizontalSlider.valueChanged.connect(self.valuechange1)
    #gaussianblur
    def valuechange2(self,value):
        img1=self.img.filter(GaussianBlur(value))
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
    def gblurtoll(self):
        horizontalSlider = QtWidgets.QSlider(self.centralwidget)
        horizontalSlider.setOrientation(QtCore.Qt.Horizontal)
        horizontalSlider.setObjectName("horizontalSlider")
        self.gridLayout_3.addWidget(horizontalSlider, 2, 2, 1, 1)
        img1=self.img.filter(GaussianBlur(1))
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
        horizontalSlider.valueChanged.connect(self.valuechange2)
    #normal blur
    def nblurtoll(self):
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        img1=self.img.filter(BLUR)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
    #bw
    def bwfun(self):
        self.delete()
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        img1=self.img.convert("L")
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)

    #filters
    def listfilters(self):
        self.delete()
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        #contour
        contour = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        contour.setObjectName("contour")
        self.gridLayout_3.addWidget(contour, 0, 0, 1, 1)
        contour.setText("contour")
        contour.clicked.connect(self.contourfilter)
        #Detail
        detail = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        detail.setObjectName("detail")
        self.gridLayout_3.addWidget(detail, 1, 0, 1, 1)
        detail.setText("detail")
        detail.clicked.connect(self.detailfilter)
        #edge_enhancer
        edge_enhancer = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        edge_enhancer.setObjectName("edge_enhancer")
        self.gridLayout_3.addWidget(edge_enhancer, 2, 0, 1, 1)
        edge_enhancer.setText("edge enhancer")
        edge_enhancer.clicked.connect(self.edge_enhancerfilter)
        #emboss
        emboss = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        emboss.setObjectName("emboss")
        self.gridLayout_3.addWidget(emboss, 0, 1, 1, 1)
        emboss.setText("emboss")
        emboss.clicked.connect(self.embossfilter)
        #find_edges
        find_edges = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        find_edges.setObjectName("find_edges")
        self.gridLayout_3.addWidget(find_edges, 0, 2, 1, 1)
        find_edges.setText("find_edges")
        find_edges.clicked.connect(self.find_edgesfilter)
        #smooth
        smooth = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        smooth.setObjectName("smooth")
        self.gridLayout_3.addWidget(smooth, 1, 1, 1, 1)
        smooth.setText("smooth")
        smooth.clicked.connect(self.smoothfilter)
        #sharper
        sharpen = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        sharpen.setObjectName("sharpen")
        self.gridLayout_3.addWidget(sharpen, 1, 2, 1, 1)
        sharpen.setText("sharpen")
        sharpen.clicked.connect(self.sharpenfilter)


    def contourfilter(self):
        img1=self.img.filter(CONTOUR)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
    def detailfilter(self):
        img1=self.img.filter(DETAIL)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
    def edge_enhancerfilter(self):
        img1=self.img.filter(EDGE_ENHANCE_MORE)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
    def embossfilter(self):
        img1=self.img.filter(EMBOSS)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
    def find_edgesfilter(self):
        img1=self.img.filter(FIND_EDGES)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
    def smoothfilter(self):
        img1=self.img.filter(SMOOTH)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
    def sharpenfilter(self):
        img1=self.img.filter(SHARPEN)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap(pixmap)
        self.img=Image.open(txt1)
    #brigthness
    def brtfun(self):
        self.delete()
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        horizontalSlider = QtWidgets.QSlider(self.centralwidget)
        horizontalSlider.setOrientation(QtCore.Qt.Horizontal)
        horizontalSlider.setObjectName("horizontalSlider")
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        enc=ImageEnhance.Brightness(self.img)
        img1= enc.enhance(1)
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap(pixmap)
        self.img=Image.open(txt1)
        self.gridLayout_3.addWidget(horizontalSlider, 0, 0, 1, 1)
        horizontalSlider.valueChanged.connect(self.brtsldr)
        horizontalSlider.setMinimum(0)
        horizontalSlider.setMaximum(20)
        horizontalSlider.setValue(10)
        horizontalSlider.setSingleStep(0.1)
    def brtsldr(self,value):
        if(value>10):
            enc=ImageEnhance.Brightness(self.img)
            img1= enc.enhance(value/10)
            img1.save(self.txt+str(self.count)+".jpg")
            txt1=self.txt+str(self.count)+".jpg"
            self.imglist.pop()
            self.imglist.append(txt1)
            pixmap = QPixmap(txt1)
            self.label.setPixmap(pixmap)
        else:
            enc=ImageEnhance.Brightness(self.img)
            img1= enc.enhance(value/10)
            img1.save(self.txt+str(self.count)+".jpg")
            txt1=self.txt+str(self.count)+".jpg"
            self.imglist.pop()
            self.imglist.append(txt1)
            pixmap = QPixmap(txt1)
            self.label.setPixmap(pixmap)
    #text
    def tpfun(self):
        self.delete()
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        self.text1= QLineEdit(self.gridLayoutWidget_3)
        self.text1.setObjectName(u"text1")
        #self.text1.setText("")

        self.gridLayout_3.addWidget(self.text1, 0, 1, 1, 1)

        self.x1 = QLineEdit(self.gridLayoutWidget_3)
        self.x1.setObjectName(u"x1")

        self.gridLayout_3.addWidget(self.x1, 1, 1, 1, 1)
        self.x1.setText("")

        self.label_2 = QLabel(self.gridLayoutWidget_3)
        self.label_2.setObjectName(u"label_2")

        self.gridLayout_3.addWidget(self.label_2, 0, 0, 1, 1)
        self.label_2.setText("enter text")

        self.label_3 = QLabel(self.gridLayoutWidget_3)
        self.label_3.setObjectName(u"label_3")

        self.gridLayout_3.addWidget(self.label_3, 1, 0, 1, 1)
        self.label_3.setText("X")

        self.label_4 = QLabel(self.gridLayoutWidget_3)
        self.label_4.setObjectName(u"label_4")

        self.gridLayout_3.addWidget(self.label_4, 2, 0, 1, 1)
        self.label_4.setText("Y")

        self.y1 = QLineEdit(self.gridLayoutWidget_3)
        self.y1.setObjectName(u"y1")

        self.gridLayout_3.addWidget(self.y1, 2, 1, 1, 1)

        self.text2 = QPushButton(self.gridLayoutWidget_3)
        self.text2.setObjectName(u"text2")

        self.gridLayout_3.addWidget(self.text2, 3, 1, 1, 1)
        self.text2.setText("add text")
        self.text2.clicked.connect(self.text1fun)
    def text1fun(self):
        #test9.test()
        mytxt=self.text1.text()
        x1=int(self.x1.text())
        y1=int(self.y1.text())
        myFont = ImageFont.truetype("arial.ttf",size=50)
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        self.count+=1
        img2=ImageDraw.Draw(self.img)
        img2.text((x1,y1),mytxt,("black"),front=myFont)
        if(self.max<self.count):
            self.max=self.count
        self.img.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap(pixmap)
        self.img=Image.open(txt1)
    #add frame
    def framefun(self):
        self.delete()
        if(self.count==0):
            self.img=Image.open("temp3.jpg")
        else:
            self.img=Image.open(self.txt+str(self.count)+".jpg")
        self.colour="black"
        self.val=0
        horizontalSlider = QtWidgets.QSlider(self.centralwidget)
        horizontalSlider.setOrientation(QtCore.Qt.Horizontal)
        horizontalSlider.setObjectName("horizontalSlider")
        self.gridLayout_3.addWidget(horizontalSlider, 0, 0, 2, 2)
        black = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        black.setObjectName("black")
        self.gridLayout_3.addWidget(black, 1, 0, 1, 1)
        black.setText("black")
        black.clicked.connect(self.setblack)
        white = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        white.setObjectName("white")
        self.gridLayout_3.addWidget(white, 1, 1, 1, 1)
        white.setText("white")
        white.clicked.connect(self.setwhite)
        red = QtWidgets.QPushButton(self.gridLayoutWidget_3)
        red.setObjectName("red")
        self.gridLayout_3.addWidget(red, 1, 2, 1, 1)
        red.setText("red")
        red.clicked.connect(self.setred)
        img1 = ImageOps.expand(self.img, border=self.val, fill=self.colour)
        self.count+=1
        if(self.max<self.count):
            self.max=self.count
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
        self.img=Image.open(txt1)
        horizontalSlider.valueChanged.connect(self.valuechange3)
    def valuechange3(self,value):
        self.val=value
        img1 = ImageOps.expand(self.img, border=value, fill=self.colour)
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
    def setred(self):
        self.colour="indianred"
        img1 = ImageOps.expand(self.img, border=self.val, fill=self.colour)
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
    def setblack(self):
        self.colour="black"
        img1 = ImageOps.expand(self.img, border=self.val, fill=self.colour)
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))
    def setwhite(self):
        self.colour="white"
        img1 = ImageOps.expand(self.img, border=self.val, fill=self.colour)
        img1.save(self.txt+str(self.count)+".jpg")
        txt1=self.txt+str(self.count)+".jpg"
        self.imglist.pop()
        self.imglist.append(txt1)
        pixmap = QPixmap(txt1)
        self.label.setPixmap((pixmap))

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "Editor"))
        self.bw.setText(_translate("MainWindow", "Black and white"))
        self.filter.setText(_translate("MainWindow", "Filters"))
        self.rotate.setText(_translate("MainWindow", "rotating "))
        self.back.setText(_translate("MainWindow", "Back"))
        self.front.setText(_translate("MainWindow", "Front"))
        self.blur.setText(_translate("MainWindow", "blur"))
        self.tp.setText(_translate("MainWindow", "Add Text"))
        self.frame.setText(_translate("MainWindow", "add frame"))
        self.brt.setText(_translate("MainWindow", "brigthness"))
        self.menuFile.setTitle(_translate("MainWindow", "File"))
        self.actionOpen.setText(_translate("MainWindow", "Open"))
        self.actionOpen.setShortcut(_translate("MainWindow", "Ctrl+O"))
        self.actionSave.setText(_translate("MainWindow", "Save"))
        self.actionSave.setShortcut(_translate("MainWindow", "Ctrl+S"))
        self.actionSave_as.setText(_translate("MainWindow", "Save as"))


if __name__ == "__main__":
    import sys
    app = QtWidgets.QApplication(sys.argv)
    MainWindow = QtWidgets.QMainWindow()
    ui = Ui_MainWindow()
    ui.setupUi(MainWindow)
    MainWindow.show()
    sys.exit(app.exec_())
