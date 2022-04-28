
# return img, nested list
def read_ppm_file(f):
    fp = open(f)
    fp.readline()  # reads P3 (assume it is P3 file)
    lst = fp.read().split()
    n = 0
    n_cols = int(lst[n])
    n += 1
    n_rows = int(lst[n])
    n += 1
    max_color_value = int(lst[n])
    n += 1
    img = []
    for r in range(n_rows):
        img_row = []
        for c in range(n_cols):
            pixel_col = []
            for i in range(3):
                pixel_col.append(int(lst[n]))
                n += 1
            img_row.append(pixel_col)
        img.append(img_row)
    fp.close()
    return img, max_color_value


# Works
def img_printer(img):
    row = len(img)
    col = len(img[0])
    cha = len(img[0][0])
    for i in range(row):
        for j in range(col):
            for k in range(cha):
                print(img[i][j][k], end=" ")
            print("\t|", end=" ")
        print()


filename = input()
operation = int(input())







#min-max normalization 
def operation_1(img,oldmax,oldmin=0):
    minimum = int(input())
    maximum = int(input())
    row = len(img)
    col = len(img[0])
    cha = len(img[0][0])
    for i in range(row):
        for j in range(col):
            for k in range(cha):
                old_val=img[i][j][k]
                img[i][j][k]= round((old_val-oldmin)/(oldmax-oldmin)*(maximum-minimum)+minimum,4)
    img_printer(img)

#z-score normalization
def operation_2(img):
    row = len(img)
    col = len(img[0])
    cha = len(img[0][0])
    rgbsumlist=[]
    standard_list=[]
    for k in range(cha):
        channelwisesum=0
        for i in range(row):
            for j in range(col):

                channelwisesum+=img[i][j][k]

        rgbsumlist.append(channelwisesum)
    # append the mean values based on their color
    redmean= (rgbsumlist[0]/row**2)
    greenmean =(rgbsumlist[1]/row**2)
    bluemean =(rgbsumlist[2]/row**2)

    for k in range(cha):
        channelstandard=0
        for i in range(row):
            for j in range(col):
                if k ==0:
                    channelstandard+=(img[i][j][k]-redmean)**2
                if k == 1:
                    channelstandard += (img[i][j][k] - greenmean) ** 2
                if k ==2:
                    channelstandard+=(img[i][j][k]-bluemean)**2
        standard_list.append(channelstandard)
    # I added 10**-6 to not face 1/0 condition
    redstandard = (standard_list[0] / row ** 2)**0.5 +10**(-6)
    greenstandard = (standard_list[1] / row ** 2)**0.5+10**(-6)
    bluestandard = (standard_list[2] / row ** 2)**0.5+10**(-6)
    # round the values to 4 digits after decimal point
    for k in range(cha):

        for i in range(row):
            for j in range(col):
                if k ==0:
                    img[i][j][k]=round((img[i][j][k]-redmean)/redstandard,4)
                if k == 1:
                    img[i][j][k] =round((img[i][j][k] - greenmean)/greenstandard,4)
                if k ==2:
                    img[i][j][k]=round((img[i][j][k]-bluemean)/bluestandard,4)

    img_printer(img)
#convert the image to black and white
def operation_3(img):
    totalsum=0
    row = len(img)
    col = len(img[0])
    cha = len(img[0][0])
    for i in range(row):
        for j in range(col):
            totalsum = 0
            for k in range(cha):
                totalsum+= img[i][j][k]
            # I made them integer by using integer division
            img[i][j]=[totalsum//3,totalsum//3,totalsum//3]
    img_printer(img)

#convolution with dimension lost
def operation_4(img,maxcolor,filter_file_name,step_move):
    row = len(img)
    col = len(img[0])
    cha = len(img[0][0])

    multiples_list=[]
    finalimg=[]

    with open(filter_file_name,"r") as f:
        for i in f.readlines():
            for j in i.strip().split():
                multiples_list.append(float(j))

    #stopindexer is the key value which indicates the necessary index after kernel matrix to for loop
    n=int(len(multiples_list)**0.5)
    stopindexer= n//2
    # If there is matrix, it has to change the values in the middle of it
    for i in range(stopindexer,row-stopindexer,step_move):
        #it is nested list so I added them individually
        collst=[]
        for j in range(stopindexer,col-stopindexer,step_move):
            charlst=[]
            for k in range(cha):
                #this kernel list changes for every single pixel,in other words we moved the matrix
                kernel_list = []
                for s in range(i-stopindexer,i+stopindexer+1):
                    for t in range(j-stopindexer,j+stopindexer+1):
                        kernel_list.append(img[s][t][k])

                value= 0
                #since we need to add all values by multiplying the same index values of matrix and real image
                for x in range(len(multiples_list)):
                    value +=kernel_list[x]*multiples_list[x]

                if value>max_color:
                    charlst.append(maxcolor)
                elif value<0:
                    charlst.append(0)
                else:
                    charlst.append(int(value))
            collst.append(charlst)
        finalimg.append(collst)

    img_printer(finalimg)

#convolution but output has the same dimensions as input image
def operation_5(img,maxcolor,filter_file_name, step_move):


    multiples_list=[]
    # I just surrounded the img list by [0,0,0] values and processed them by op 4 func

    with open(filter_file_name,"r") as f:
        for i in f.readlines():
            for j in i.strip().split():
                multiples_list.append(float(j))

    n=int(len(multiples_list)**0.5)
    stopindexer= n//2
    total_length=len(img)+2*stopindexer
    zero_line = [[0, 0, 0]]* (total_length)
    for i in range (stopindexer):
        img.insert(0, zero_line)

    for i in range(stopindexer, len(img)):
        for x in range(stopindexer):
            img[i].insert(0, [0, 0, 0])
            img[i].append([0, 0, 0])

    for i in range(stopindexer):
        img.append(zero_line)



    operation_4(img,maxcolor,filter_file_name,step_move)




#color quantization
def operation_6(img,range1,sidelength,row=0,col=0,rgbvaluememo=[0,0,0],direction="down" ):
    #if row <0 this means that we are at the top of img

    if row <0:
        if col<sidelength-1:
            return operation_6(img,range1,sidelength,row+1,col+1,rgbvaluememo,"down")
        else:
            img_printer(img)
            return

    #bottom
    if row ==sidelength:
        if col< sidelength-1:
            #by changing direction string I achieved snakelike move
            return operation_6(img,range1,sidelength,row-1,col+1,rgbvaluememo,"up")
        else:
            img_printer(img)
            return

    # if it is close then change the value
    isclosedown=True
    for i in range(3):
        if img[row][col][i]>= rgbvaluememo[i]:
            if img[row][col][i]- rgbvaluememo[i] >= range1:
                isclosedown = False

        else:
            if rgbvaluememo[i]-img[row][col][i] >= range1:
                isclosedown = False


    if isclosedown==True:
        img[row][col]=rgbvaluememo


    if direction == "down":
        operation_6(img, range1, sidelength, row + 1, col, img[row][col])
    else:
        operation_6(img, range1, sidelength, row - 1, col, img[row][col], "up")


#color quantization but this time it is a 3d quantization
def operation_7(img,range1,sidelength,row=0,col=0,r=0,chaindex=0,direction="down"):
    #same func as op 6 but this time it might go backwards so if it is 0,2 which is r,b go straight forward, else come backwards
    if chaindex%2==0:
        if row <0:
            if col<sidelength-1:

                return operation_7(img,range1,sidelength,row+1,col+1,img[row+1][col][chaindex],chaindex, "down",)
            else:

                if chaindex==3:
                    img_printer(img)
                    return
                else:
                    return operation_7(img, range1, sidelength, row+1, col ,img[row+1][col][chaindex], chaindex+1,"down")



        if row ==sidelength:
            if col< sidelength-1:

                return operation_7(img,range1,sidelength,row-1,col+1,img[row-1][col][chaindex],chaindex,"up")
            else:
                if chaindex == 3:
                    img_printer(img)
                    return
                else:
                    return operation_7(img, range1, sidelength, row-1, col, img[row-1][col][chaindex], chaindex+1, "up")


        if chaindex==3:
            img_printer(img)
            return
        isr=True




        if img[row][col][chaindex]>= r:
            if img[row][col][chaindex]- r >= range1:
                isr = False

        else:
            if r -img[row][col][chaindex] >= range1:
                isr= False


        if isr==True:
            img[row][col][chaindex]=r



        if direction == "down":

            operation_7(img, range1, sidelength, row + 1, col, img[row][col][chaindex],chaindex)
        else:
            operation_7(img, range1, sidelength, row - 1, col,img[row][col][chaindex],chaindex, "up")

    else:

        if row < 0:
            if 0 < col :

                return operation_7(img, range1, sidelength, row + 1, col -1, img[row + 1][col][chaindex], chaindex,"down" )
            else:

                if chaindex == 3:
                    img_printer(img)
                    return
                else:

                    return operation_7(img, range1, sidelength, row+1, col, img[row+1][col][chaindex],chaindex + 1, "down")

        if row == sidelength:
            if 0 <col :

                return operation_7(img, range1, sidelength, row - 1, col - 1, img[row - 1][col][chaindex], chaindex,"up")
            else:
                if chaindex == 3:
                    img_printer(img)
                    return
                else:
                    return operation_7(img, range1, sidelength, row-1, col, img[row-1][col][chaindex], chaindex + 1, "up")

        if chaindex == 3:
            img_printer(img)
            return
        isr = True



        if img[row][col][chaindex] >= r:
            if img[row][col][chaindex] - r >= range1:
                isr = False

        else:
            if r - img[row][col][chaindex] >= range1:
                isr = False


        if isr == True:
            img[row][col][chaindex] = r



        if direction == "down":

            operation_7(img, range1, sidelength, row + 1, col, img[row][col][chaindex], chaindex)
        else:
            operation_7(img, range1, sidelength, row - 1, col, img[row][col][chaindex], chaindex, "up")







img, max_color=read_ppm_file(filename)

#calling the functions


if operation==1:
    operation_1(img,max_color)

if operation==2:
    operation_2(img)

if operation==3:
    operation_3(img)

if operation==4:
    filter_file_name=input()
    step_move=int(input())
    operation_4(img,max_color,filter_file_name,step_move)
if operation==5:
    filter_file_name=input()
    step_move=int(input())
    operation_5(img,max_color,filter_file_name,step_move)
if operation==6:
    n=int(input())
    side_length= len(img)

    operation_6(img,n,side_length)

if operation==7:
    n=int(input())
    side_length=len(img)
    operation_7(img,n,side_length)





