## Solving the n-queen puzzle with the simulated annealing heuristic

**What is simulated annealing?**

Simulated annealing is a metaheuristic used to solve optimization problems. Its main idea comes from the physical process of alternating between slowly cooling down a mterial and reheating it, minimising its energy. 
This graph below shows the general concept of the simulated annealing metaheuristic that we will adapt to the n-queen puzzle to find a (hopefully) optimal solution. 


![alt text](https://github.com/Nehaila/nqueen-SA/blob/master/sa.jpg)

To adopt this to the n-queen problem, we start by an initial temperature T that we slowly decrease at the end of each iteration, the candidate solution in this case will mean permuting two columns of the puzzle board, calculating its cost (how many interacting queens we have), if the cost is lower we will keep the solution, else we will decide whether to keep the solution with a probability. 
The program stops once it finds an optimal solution or once the temperature T reaches 0.

If you're not familiar with simulated annealing and want to try the app you can try the following parameters: 
* Board size: 7
* Mode: Random 
Then click on initial solution 
* initial T: 1000 
* imax: 20 
* Alpha: 0,9 
* Number of transformations with fixed T: 100
* Execute: complete


The code in written with VBA- Excel, here's a snippet of the code used to actually execute the simulated annealing, (you can find the complete code by downloading the app):

``` Private Sub CommandButton2_Click()
UserForm1.Hide 

Dim secs1 As Single

secs1 = Timer()
Dim secs2 As Single
Dim Ti As Single
Dim alpha As Double
Dim proba As Single
Dim nbr As Single
Dim T As Double
Dim col1 As Integer
Dim col2 As Integer
Dim Energie1 As Integer
Dim Energie As Integer
Dim val As Integer
Dim En As Integer

'Dim c As Chart
n = TextBox1.Text
Ti = TextBox2.Text
imax = TextBox3.Text
alpha = TextBox5.Text
Trsf = TextBox4.Text
Energie = count()
En = Energie
        Sheets("Feuil1").Range("N5").Value = En

T = Ti
While T >= 0.001:
    If OptionButton8.Value = True Then
        For i = 1 To imax
            Sheets("Feuil1").Range("N8").Value = i
            If Energie = 0 Then
                En = Energie
                Sheets("Feuil1").Range("N5").Value = En
                Sheets("Feuil1").Range("N4").Value = Energie
                Exit For
                Sheets("Feuil1").Visible = True
            End If
            Sheets("Feuil1").Range("N6").Value = T
            For Y = 1 To Trsf
                Randomize
                Random = Rnd
                col1 = Int(Rnd * n) + 1
                col2 = Int(Rnd * n) + 1
                Delta = Permute(col1, col2)
                Energie1 = count()
                Sheets("Feuil1").Range("N4").Value = Energie
                proba = Exp(-((Energie1 - Energie)) / (T + 0.01))
                If Energie1 = 0 Then
                    Energie = Energie1
                    En = Energie
                    Sheets("Feuil1").Range("N5").Value = En
                    Sheets("Feuil1").Range("N4").Value = Energie
                    Exit For
                End If
                If Energie1 < Energie Then
                    Energie = Energie1
                    Sheets("Feuil1").Range("N4").Value = Energie
                    If Energie < En Then
                        En = Energie
                        Sheets("Feuil1").Range("N5").Value = En
                    End If
                ElseIf Energie1 - Energie >= 0 Then
                    If Random >= proba Then
                        Delta = Permute(col1, col2)
                        Energie = count()
                    ElseIf Random < proba Then
                       Energie = Energie1
                    End If
                End If
            Next Y
        secs2 = Timer()
        Sheets("Feuil1").Range("N7").Value = secs2 - secs1
        Next i
T = (alpha) * T
MsgBox ("Temps Total :" & vbNewLine & secs2 - secs1 & " seconds")
    ElseIf OptionButton7.Value = True Then
        i = 1
        Do While i <= imax
            Sheets("Feuil1").Range("N8").Value = i
            If Energie = 0 Then
                En = Energie
                Sheets("Feuil1").Range("N5").Value = En
                Sheets("Feuil1").Range("N4").Value = Energie
                Exit Do
            End If
        Sheets("Feuil1").Range("N6").Value = T
        For Y = 1 To Trsf
            Randomize
            answer = MsgBox(" Le cout actuel est" & " " & Energie & " " & "Continuer?", vbYesNo + vbQuestion, i = i + 1)
            col1 = Int(Rnd * n) + 1
            col2 = Int(Rnd * n) + 1
            Delta = Permute(col1, col2)
            Energie1 = count()
            proba = Exp(-((Energie1 - Energie)) / (T + 0.1))
            Random = Rnd
            If Energie1 = 0 Then
                Energie = Energie1
                En = Energie
                Sheets("Feuil1").Range("N5").Value = En
                Exit For
            End If
            If Energie1 < Energie Then
                Energie = Energie1
                Sheets("Feuil1").Range("N4").Value = Energie
                If Energie < En Then
                    En = Energie
                    Sheets("Feuil1").Range("N5").Value = En
                End If
            ElseIf Energie1 - Energie >= 0 Then
                If Random > proba Then
                    Delta = Permute(col1, col2)
                    Energie = count()
                ElseIf Random <= proba Then
                Energie = Energie1
                End If
            End If
            Sheets("Feuil1").Range("N5").Value = En
            Sheets("Feuil1").Range("N4").Value = Energie
        Next Y
T = (alpha) * T
secs2 = Timer()
Sheets("Feuil1").Range("N7").Value = secs2 - secs1
Loop
End If
End Sub 
```

