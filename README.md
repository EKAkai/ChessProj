# ChessProj
Semicomplete chess program 
unit UBoards;
interface
uses Controls;
type  TPieceType = (BlackKing, WhiteKing, WhiteBishop, WhiteRook, BlackKnight,    WhitePawn, BlackPawn, WhiteQueen, BlackQueen, BlackRook, BlackBishop,    WhiteKnight);
  TChessPiece = class(TGraphicControl)  protected    FColour: string;    FX: integer;    FY: integer;    FType: TPieceType;    FTaken: boolean;    BPiece: TChessPiece;  public    Constructor Create(x, y: integer; colour: string; Name: TPieceType;      isTaken: boolean);    Destructor Destroy;    function IsValid(OrigLocX, OrigLocY, SqrLocX, SqrLocY: integer): boolean;      virtual; abstract;    procedure Move(NewX, NewY: integer);    function Guessplace(PieceLocationX, PieceLocationY: integer)      : integer; virtual;    function Checkmate(PieceLocationX, PieceLocationY: integer)      : boolean; virtual;    property x: integer read FX write FX;    property y: integer read FY write FY;    property PType: TPieceType read FType;    property Taken: boolean read FTaken write FTaken;    property Colour:string read FColour;
  end;
  TBoardArray = array [0 .. 7, 0 .. 7] of TChessPiece;
  TQueen = class(TChessPiece)
  public    function IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean; override;    function Guessplace(PieceLocationX, PieceLocationY: integer)      : integer; override;  end;
  TKing = class(TChessPiece)  public    function IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;    function Guessplace(PieceLocationX, PieceLocationY: integer)      : integer; override;  end;
  TRook = class(TChessPiece)  public    function IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;    function Guessplace(PieceLocationX, PieceLocationY: integer)      : integer; override;  end;
  TBishop = class(TChessPiece)  public    function IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;    function Guessplace(PieceLocationX, PieceLocationY: integer)      : integer; override;  end;
  TKnight = class(TChessPiece)  public    function IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;    function Guessplace(PieceLocationX, PieceLocationY: integer)      : integer; override;  end;
  TPawn = class(TChessPiece)  public    function IsValid(OrigLocX, OrigLocY, SqrLocX, SqrLocY: integer): boolean;    function Guessplace(PieceLocationX, PieceLocationY: integer)      : integer; override;  end;
  TBoard = class  protected    Favaliable: boolean;    FSpaces: TBoardArray;    IsSelected: boolean;  public    Constructor Create;    function isAvaliable(x,y,nx, ny: integer): boolean;    procedure Move(OriginalX, OriginalY, NewX, NewY: integer);    property Spaces: TBoardArray read FSpaces write FSpaces;    property Select: boolean read IsSelected write IsSelected;    property Avaliable: boolean read Favaliable write Favaliable;  end;
implementation
{ TBoard }
constructor TBoard.Create;var  i, j, k: integer;begin
  FSpaces[0, 0] := TRook.Create(0, 0, 'white', WhiteRook, false);  FSpaces[0, 7] := TRook.Create(0, 7, 'white', WhiteRook, false);  FSpaces[0, 1] := TKnight.Create(0, 1, 'white', WhiteKnight, false);  FSpaces[0, 6] := TKnight.Create(0, 6, 'white', WhiteKnight, false);  FSpaces[0, 2] := TBishop.Create(0, 2, 'white', WhiteBishop, false);  FSpaces[0, 5] := TBishop.Create(0, 5, 'white', WhiteBishop, false);  FSpaces[0, 3] := TQueen.Create(0, 3, 'white', WhiteQueen, false);  FSpaces[0, 4] := TKing.Create(0, 4, 'white', WhiteKing, false);  FSpaces[7, 0] := TRook.Create(7, 0, 'black', BlackRook, false);  FSpaces[7, 7] := TRook.Create(7, 7, 'black', BlackRook, false);  FSpaces[7, 1] := TKnight.Create(7, 1, 'black', BlackKnight, false);  FSpaces[7, 6] := TKnight.Create(7, 6, 'black', BlackKnight, false);  FSpaces[7, 2] := TBishop.Create(7, 2, 'black', BlackBishop, false);  FSpaces[7, 5] := TBishop.Create(7, 5, 'black', BlackBishop, false);  FSpaces[7, 3] := TQueen.Create(7, 3, 'black', BlackQueen, false);  FSpaces[7, 4] := TKing.Create(7, 4, 'black', BlackKing, false);
  for i := 0 to 7 do  begin    FSpaces[1, i] := TPawn.Create(1, i, 'white', WhitePawn, false);    FSpaces[6, i] := TPawn.Create(6, i, 'black', BlackPawn, false);  end;
end;
function TBoard.isAvaliable(x,y,nx, ny: integer): boolean; var PlayerStart:string; PlayerEnd:string; TypetoPlay:TPieceType; TypetoMove:TPieceType;beginTypetoPlay:=FSpaces[x,y].PType;TypetoMove:=FSpaces[nx,ny].PType;if FSpaces[x,y]<> nil thenbeginif (TypetoPlay)=(BlackKing) or (BlackQueen) or (BlackBishop) or (BlackPawn) or (BlackKnight) or (BlackRook) then begin   PlayerStart:='Player2'; end else begin   PlayerStart:='Player1'; end; if (TypetoMove)= BlackKing or BlackQueen or BlackBishop or BlackPawn or BlackKnight or BlackRook then begin   PlayerEnd:='Player2'; end else begin   PlayerEnd:='Player1'; end;
end;
 if FSpaces[nx, ny] <> nil then  begin   if (PlayerEnd)=(PlayerStart) then   begin   result:=false;   end   else   begin     FSpaces[nx,ny].Taken:=true;   end;  end  else  begin    result:=false;  end;

end;
procedure TBoard.Move(OriginalX, OriginalY, NewX, NewY: integer);begin  FSpaces[OriginalX, OriginalY].Move(NewX, NewY);  FSpaces[NewX, NewY] := FSpaces[OriginalX, OriginalY];  FSpaces[OriginalX, OriginalY] := nil;end;
{ TKing }
function TKing.Guessplace(PieceLocationX, PieceLocationY: integer): integer;begin
end;
function TKing.IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;begin
end;
{ TChessPiece }
function TChessPiece.Checkmate(PieceLocationX, PieceLocationY: integer)  : boolean;begin  { if TKing.Taken Then    result := TRUE; }
end;
constructor TChessPiece.Create(x, y: integer; colour: string; Name: TPieceType;  isTaken: boolean);begin  x := FX;  y := FY;  colour := FColour;  FType := Name;  isTaken := Taken;end;
destructor TChessPiece.Destroy;begin  BPiece.Free;end;
function TChessPiece.Guessplace(PieceLocationX, PieceLocationY  : integer): integer;begin
end;
procedure TChessPiece.Move(NewX, NewY: integer);begin  FX := NewX;  FY := NewY;end;
{ TQueen }
function TQueen.Guessplace(PieceLocationX, PieceLocationY: integer): integer;begin
end;
function TQueen.IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;begin
end;
{ TRook }
function TRook.Guessplace(PieceLocationX, PieceLocationY: integer): integer;begin
end;
function TRook.IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;begin
end;
{ TBishop }
function TBishop.Guessplace(PieceLocationX, PieceLocationY: integer): integer;begin
end;
function TBishop.IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;begin
end;
{ TKnight }
function TKnight.Guessplace(PieceLocationX, PieceLocationY: integer): integer;begin
end;
function TKnight.IsValid(OrigLocX,OrigLocY,SqrLocX, SqrLocY: integer): boolean;begin
end;
{ TPawn }
function TPawn.Guessplace(PieceLocationX, PieceLocationY: integer): integer;begin
end;
function TPawn.IsValid(OrigLocX, OrigLocY, SqrLocX, SqrLocY: integer): boolean;begin  if SqrLocX = OrigLocX + 1 then  begin    result := true  end  else  begin    result := false;  end;
end;
end.
