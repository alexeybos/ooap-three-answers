На основе оставшихся классов получились следующие кластеры:
core: PlayingProcess (или GameEngine)
field: GameField, GameFieldElement, FieldElementCollection
rules: Combinations
control: CommandHandler, GameCommand и соответственно конкретные команды (UserTurnCommand, UndoCommand(?), ExitCommand, ReRunGameCommand)
data (или stats): GameStatistics, EventObserver, наблюдатель ScoreCounter, наблюдатель HistoryWriter, наблюдатель GameStatistics 

Вообще, все больше и больше кажется, что иду (скажем мягко) не очень верным путем. Но тут, наверное, надо однозначно упереться в уже явный тупик, 
чтобы по факту можно было проанализировать свои шаги, которые туда и привели...    