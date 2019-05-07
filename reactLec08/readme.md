# (Todo 만들기) 기능 추가

### change / insert

- handleChange 메서드 추가
- handleInsert 메서드 추가
- handleKeyDown 메서드 추가

```shell
git commit -m 'add change/insert'
```

### remove

- handleRemove 메서드 추가
- handleRemove / onRemove props 연결

```shell
git commit -m 'add remove'
```

### toggle

- handleToggle 메서드 추가
- handleToggle / onToggle 연결
- is-done 클래스 추가

```shell
git commit -m 'add toggle'
```

### toggleAll

- handleToggleAll 메서드 추가
- isAllDone prop 추가
- handleToggleAll / onToggleAll 연결

```shell
git commit -m 'add toggleAll`
```

### edit start/save/cancel

- editingId state 추가
- handleEditStart 메서드 추가
- handleEditSave 메서드 추가
- handleEditCancel 메서드 추가
- handleEditStart/handleEditSave/handleEditCancel
- onEditStart/onEditSave/onEditCancel 연결
- isEditing 클래스 추가
- edit input에 ref추가
- componentDidUpdate에서 prop 비교 / foucs()

### changeFilter / clearCompleted

- filterName state 추가
- handleFilterChange 메서드 추가
- handleClearCompleted 메서드 추가
- filteredTodos prop 추가
- activeLength prop 추가
- shouldClearCompletedShow prop 추가
- filterItems 매핑
- activeLength 연결
- shouldClearCompletedShow 연결

```shell
git commit -m 'add changeFilter/clearCompleted'
```