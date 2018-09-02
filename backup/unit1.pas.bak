unit Unit1;

{$mode objfpc}{$H+}

interface

uses
  Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
  ExtCtrls, Spin, wincrt, graph;
type

  { TForm1 }

  TForm1 = class(TForm)
    FirstMoveCheckBox: TCheckBox;
    InvalidMoveLabel: TLabel;
    GameOverLabel: TLabel;
    Label3: TLabel;
    NewGameButton: TButton;
    Circle21: TShape;
    Circle42: TShape;
    Circle43: TShape;
    Circle44: TShape;
    Circle34: TShape;
    Circle24: TShape;
    Circle14: TShape;
    Circle45: TShape;
    Circle35: TShape;
    Circle25: TShape;
    Circle15: TShape;
    Circle12: TShape;
    Circle51: TShape;
    Circle52: TShape;
    Circle53: TShape;
    Circle54: TShape;
    Circle55: TShape;
    Circle61: TShape;
    Circle62: TShape;
    Circle63: TShape;
    Circle64: TShape;
    Circle65: TShape;
    Circle22: TShape;
    Circle16: TShape;
    Circle26: TShape;
    Circle36: TShape;
    Circle46: TShape;
    Circle56: TShape;
    Circle66: TShape;
    Circle17: TShape;
    Circle27: TShape;
    Circle37: TShape;
    Circle47: TShape;
    Circle13: TShape;
    Circle57: TShape;
    Circle67: TShape;
    Circle23: TShape;
    Circle31: TShape;
    Circle32: TShape;
    Circle33: TShape;
    Circle41: TShape;
    Label1: TLabel;
    Panel: TPanel;
    LevelSpin: TSpinEdit;
    TShape1: TShape;
    TShape10: TShape;
    TShape33: TShape;
    TShape12: TShape;
    TShape24: TShape;
    TShape34: TShape;
    TShape14: TShape;
    TShape41: TShape;
    TShape42: TShape;
    TShape43: TShape;
    TShape44: TShape;
    TShape2: TShape;
    TShape45: TShape;
    TShape51: TShape;
    TShape52: TShape;
    TShape53: TShape;
    TShape54: TShape;
    TShape55: TShape;
    TShape61: TShape;
    TShape62: TShape;
    TShape63: TShape;
    TShape64: TShape;
    TShape3: TShape;
    TShape65: TShape;
    TShape16: TShape;
    TShape26: TShape;
    TShape36: TShape;
    TShape46: TShape;
    TShape56: TShape;
    TShape66: TShape;
    TShape17: TShape;
    TShape27: TShape;
    TShape37: TShape;
    TShape4: TShape;
    TShape47: TShape;
    TShape57: TShape;
    TShape67: TShape;
    Circle11: TShape;
    TShape5: TShape;
    TShape6: TShape;
    TShape25: TShape;
    TShape35: TShape;
    TShape15: TShape;
    procedure FormCreate(Sender: TObject);
    procedure LevelSpinChange(Sender: TObject);
    procedure NewGameButtonClick(Sender: TObject);
    procedure ClickHandler(Sender: TObject);
    procedure EndGame(endValue : integer);
  private
    { private declarations }
  public
    { public declarations }
  end;


type
  GameBoard = array[1..6, 1..7] of char;

  GameMove = array[1..2] of shortint;

var
  Form1: TForm1;
  e : integer;
  gameposition: GameBoard;
  inputColumn, inputDepth, i, j, depth, row, column : shortint;
  graphicsMode, graphicsDriver, rowStart, columnStart : smallint;
  computerMove : GameMove;
  computerTurn, firstToMove, gameInProgress : boolean;
  moveOrder : array[1..7] of shortint;
  formBoard : array[1..7] of TShape;


implementation

{$R *.lfm}

procedure output;
var
  i, x, y : integer;
  name : string;
  shape : TShape;
begin
  for i := 0 to Form1.Panel.ControlCount - 1 do
  begin
    shape := Form1.Panel.Controls[i] as TShape;
    name := shape.Name;
    if Copy(name, 1, 6) = 'Circle' then
    begin
      y := StrToInt(Copy(name, 7, 1));
      x := StrToInt(Copy(name, 8, 1));
      if gameposition[y, x] = 'O' then
      begin
        shape.Visible := true;
        shape.Brush.Color := clRed;
      end
      else if gameposition[y, x] = 'X' then
      begin
        shape.Visible := true;
        shape.Brush.Color := clYellow;
      end
      else
      begin
        shape.Visible := false;
      end;
    end;
  end;
end;

procedure clearboard;
var x, y : integer;
begin
  for x := 1 to 7 do
  begin
    for y := 1 to 6 do
    begin
      gameposition[y, x] := '_'
    end;
  end;
  output;
end;

function makeMove(column: shortint; character: char;
    currentPosition: GameBoard): GameBoard;
  var
    x: integer;
  begin
    for x := 1 to 6 do
    begin
      if currentPosition[7 - x, column] = '_' then
      begin
        currentPosition[7 - x, column] := character;
        Exit(currentPosition);
      end;
    end;
  end;

function isGameOver(currentPosition : GameBoard): shortint;
  var
    x, y : shortint;
    isDraw : boolean;
  begin

    for x := 1 to 3 do
    begin
      for y := 1 to 4 do //checks for wins in diagonals
      begin
         if (currentPosition[x, y] <> '_') and
           (currentPosition[x, y] = currentPosition[x + 1, y + 1]) and
           (currentPosition[x, y] = currentPosition[x + 2, y + 2]) and
           (currentPosition[x, y] = currentPosition[x + 3, y + 3]) then
           begin
             if currentPosition[x, y] = 'X' then
                 Exit(0)
             else
                 Exit(3);
           end;
           if (currentPosition[7 - x, y] <> '_') and
           (currentPosition[7 - x, y] = currentPosition[6 - x, y + 1]) and
           (currentPosition[7 - x, y] = currentPosition[5 - x, y + 2]) and
           (currentPosition[7 - x, y] = currentPosition[4 - x, y + 3]) then
           begin
             if currentPosition[7 - x, y] = 'X' then
                 Exit(0)
             else
                 Exit(3);
           end;
      end;
    end;

    for x := 1 to 3 do //a win in the columns
    begin
      for y := 1 to 7 do
      begin
         if (currentPosition[x, y] <> '_') and
           (currentPosition[x, y] = currentPosition[x + 1, y]) and
           (currentPosition[x, y] = currentPosition[x + 2, y]) and
           (currentPosition[x, y] = currentPosition[x + 3, y]) then
           begin
             if currentPosition[x, y] = 'X' then
                 Exit(0)
             else if currentPosition[x, y] = 'O' then
                 Exit(3);
           end;
      end;
    end;

    for x := 1 to 6 do
    begin
      for y := 1 to 4 do
      begin
         if (currentPosition[x, y] <> '_') and
           (currentPosition[x, y] = currentPosition[x, y + 1]) and
           (currentPosition[x, y] = currentPosition[x, y + 2]) and
           (currentPosition[x, y] = currentPosition[x, y + 3]) then
           begin
             if currentPosition[x, y] = 'X' then
                 Exit(0)
             else
                 Exit(3);
           end;
      end;
    end;

    for x := 1 to 3 do
    begin
      for y := 1 to 7 do
      begin
         if (currentPosition[x, y] <> '_') and
           (currentPosition[x, y] = currentPosition[x + 1, y]) and
           (currentPosition[x, y] = currentPosition[x + 2, y]) and
           (currentPosition[x, y] = currentPosition[x + 3, y]) then
           begin
             if currentPosition[x, y] = 'X' then
                 Exit(0)
             else
                 Exit(3);
           end;
      end;
    end;
    isDraw := true;
    for y := 1 to 7 do
    begin
      if currentPosition[1, y] = '_' then
         isDraw := false;
  end;
  if isDraw = true then
     Exit(1)
  else
    Exit(2);
end;

function threatValue(pos : GameBoard; x : shortint;
  y : shortint) : shortint;
var //This function will probably the longest part of the evaluation, but
threatScore, isThreat : shortint;
begin  //will improve the program's strength by far
  threatScore := 10;

  if pos[x, y] <> '_' then
     Exit(0);

  pos[x, y] := 'X';
  isThreat := isGameOver(pos);
  if isThreat = 0 then
     Exit(-threatScore);

  pos[x, y] := 'O';
  isThreat := isGameOver(pos);
  if isThreat = 3 then
     Exit(threatScore);
  threatValue := 0;
end;

function Evaluate(currentPosition: GameBoard): shortint;
var score, value, x, y : shortint;
begin
     score := 0;
     e += 1;
     for x := 1 to 6 do
     begin
       for y := 1 to 7 do
       begin
       value := threatValue(currentPosition, x, y);
       if value <> 0 then begin
          if ((firstToMove = true) xor (value < 0)) and ((x mod 2) = 1) or
             ((firstToMove = false) xor (value < 0)) and ((x mod 2) = 0) then
                value *= 2;
          if x = 6 then
             score += value
          else if value = threatValue(currentPosition, x + 1, y) then
             score += value * 5
          else if not (value = -threatValue(currentPosition, x + 1, y)) then
             score += value;
          end;
       end;
     end;
     for x := 1 to 6 do
     begin
       if currentPosition[x, 4] = 'O' then
         score += 5
       else if currentPosition[x, 4] = 'X' then
         score -= 5;
     end;
     Evaluate := score;
end;

function findBestMove(currentPosition: GameBoard; currentDepth: shortint;
  compTurn: boolean; alpha : shortint): GameMove;
var
  y, moveValue, rawScore, column: shortint;
  positionToEvaluate: GameBoard;
  bestMove, analysedMove: GameMove;
  character: char;
begin
  if compTurn = true then
  begin
    character := 'X';
    bestMove[2] := 100;
  end
  else
  begin
    character := 'O';
    bestMove[2] := -100;
  end;
    bestMove[1] := 0;

    for y := 1 to 7 do
    begin
      column := moveOrder[y];
      if currentPosition[1, column] = '_' then
      begin
        positionToEvaluate := makeMove(column, character, currentPosition);
        rawScore := isGameOver(positionToEvaluate);
        if (currentDepth = depth) or (rawScore <> 2) then
        begin
             case rawScore of
              -1 : moveValue := -101 + currentDepth;
               0 : moveValue := -101 + currentDepth;
               1 : moveValue := 0;
               2 : moveValue := Evaluate(positionToEvaluate);
               3 : moveValue := 101 - currentDepth;
               4 : moveValue := 101 - currentDepth;
             end;
        end
        else
        begin
          analysedMove := findBestMove(positionToEvaluate,
             currentDepth + 1, not compTurn, bestMove[2]);
          moveValue := analysedMove[2];
          if ((moveValue < alpha) and (compTurn = true)) or
             ((moveValue > alpha) and (compTurn = false)) then
             Exit(analysedMove);
        end;


        if ((compTurn = true) and (moveValue < bestMove[2])) or
          ((compTurn = false) and (moveValue > bestMove[2])) or
          (not (bestMove[1] in [1..7])) then
        begin
          bestMove[1] := column;
          bestMove[2] := moveValue;
        end;
      end;
  end;
  findBestMove := bestMove;
end;

{ TForm1 }

procedure TForm1.EndGame(endValue : integer);
begin
  gameInProgress := false;

  case endValue of
   -1: GameOverLabel.Caption := 'Game over - Computer wins';
    0: GameOverLabel.Caption := 'Game over - Computer wins';
    1: GameOverLabel.Caption := 'Game over - Draw';
    3: GameOverLabel.Caption := 'Game over - You win';
    4: GameOverLabel.Caption := 'Game over - You win';
  end;
end;

procedure MakeComputerMove;
var themove : GameMove;
begin
  Form1.GameOverLabel.Caption := 'Computer''s turn';
  Application.ProcessMessages;
  themove := findBestMove(gameposition, 1, true, -127);
  gameposition := makeMove(themove[1], 'X', gameposition);
  output;
  if isGameOver(gamePosition) <> 2 then
  begin
    Form1.EndGame(isGameOver(gamePosition));
  end
  else
  begin
    Form1.GameOverLabel.Caption := 'Your turn';
  end;
end;

procedure TForm1.NewGameButtonClick(Sender: TObject);
begin
  clearboard;
  gameInProgress := true;
  firstToMove := FirstMoveCheckBox.Checked;
  if not firstToMove then
  begin
    MakeComputerMove;
  end;
end;

procedure TForm1.ClickHandler(Sender: TObject);
var cpos : TPoint;
  x : integer;
begin
  if not gameInProgress then
    Exit;
  InvalidMoveLabel.Visible := false;
  cpos := ScreenToClient(Mouse.CursorPos);
  x := (cpos.x - Panel.Left) div 70 + 1;
  if gameposition[1, x] = '_' then
  begin
    gameposition := makeMove(x, 'O', gameposition);
    output;
    if isGameOver(gamePosition) <> 2 then
      Form1.EndGame(isGameOver(gamePosition))
    else
      makecomputermove;
  end
  else
  begin
    InvalidMoveLabel.Visible := true;
  end;
end;

procedure TForm1.FormCreate(Sender: TObject);
var shape: TControl;
begin
  clearboard;
  for i := 0 to Panel.ControlCount - 1 do
  begin
    shape := Panel.Controls[i];
    shape.OnClick := @ClickHandler;
  end;
  depth := Form1.LevelSpin.Value;
end;

procedure TForm1.LevelSpinChange(Sender: TObject);
begin
  depth := LevelSpin.Value;
end;

begin
  moveOrder[1] := 4;
  moveOrder[2] := 3;
  moveOrder[3] := 5;
  moveOrder[4] := 2;
  moveOrder[5] := 6;
  moveOrder[6] := 1;
  moveOrder[7] := 7;
  gameInProgress := true;
end.

