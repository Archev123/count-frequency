MinTime=20  #set the minimum frequency for word

file=open("PointerFinish.txt","r+")
CountOutput=open("CountOutput.txt","wb")
FinalOutputWithMerge=open("FinalOutputWithMerge.txt","wb")

wordcount={}
pre_wordcount={}
fin_wordcount={}


for word in file.read().split():
    if word not in wordcount:
        wordcount[word] = 1
    else:
        wordcount[word] += 1

fin_word=[]
fin_fre=[]
pre_word=[]
pre_fre=[]

for k,v in wordcount.items():
    CountOutput.write(str(k) + "  " + str(v) + " ")
    if v<MinTime:
        pre_word.append(k)
        pre_fre.append(v)
    else:
        fin_word.append(k)
        fin_fre.append(v)
CountOutput.close()

fin_coordinates=[]

for i in range(len(fin_word)):
    temp_array = []
    x=float(fin_word[i])
    loop1 = x
    while loop1 != 0:
        y = int(x % 10)
        temp_array.append(y)
        x = int(x / 10)
        loop1 = int(x % 10)
    fin_coordinates.append(temp_array)


#----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

word_index=[]
for i in range(len(pre_word)):
    temp_array=[]
    x = float(pre_word[i])
    loop1 = 1
    N = 0
    while loop1 != 0:
        y = int(x % 10)
        temp_array.append(y)
        loop2 = float(x / 10)
        x = int(x/10)
        N = N + 1
        temp_index=0
        temp_dis=0
        min_dis = 999999
        while loop2 == 0:
            for j in range(len(fin_word)):
                for k in range(N):
                    temp_dis = temp_dis + (temp_array[k] - fin_coordinates[j][k]) ** 2
                if temp_dis <= min_dis:
                    temp_index=j
                    min_dis = temp_dis
                temp_dis = 0
                loop2 =1
            word_index.append(temp_index)
            temp_index = 0
            min_dis = 999999
            loop1 =0

test_fre = []
for i in range(len(fin_fre)):
    test_fre.append(int(fin_fre[i]))
#----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
for i in range(len(word_index)):
    x = int(word_index[i])-1
    test_fre[x]=int(fin_fre[x])+int(pre_fre[i])

fin_wordcount=[fin_word,test_fre]

for i in range(len(fin_word)):
    x=test_fre[i]
    for j in range(x):
        FinalOutputWithMerge.write(str(fin_word[i]) + " ")

FinalOutputWithMerge.close()
