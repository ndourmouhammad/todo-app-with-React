# Notes sur React - Apprentissage par l'exemple (Todo App)

## 1. Les Composants React

### Qu'est-ce qu'un composant ?
Un composant React est une fonction JavaScript qui retourne du JSX (JavaScript XML). Dans notre Todo App, nous avons plusieurs composants :

```jsx
// Exemple de composant simple (Header.jsx)
export function Header(props) {
    return <h1>Todo App</h1>
}
```

### Types de composants
1. **Composants Fonctionnels** (utilisés dans notre projet)
   ```jsx
   function TodoCard(props) {
       return <div>...</div>
   }
   ```

2. **Composants de Classe** (non utilisés dans ce projet)
   ```jsx
   class TodoCard extends React.Component {
       render() {
           return <div>...</div>
       }
   }
   ```

## 2. Les Props

Les props sont des données passées d'un composant parent à un composant enfant.

### Exemple dans notre Todo App :
```jsx
// Dans App.jsx (parent)
<TodoList 
    todos={todos} 
    selectedTab={selectedTab} 
    handleCompleteTodo={handleCompleteTodo} 
/>

// Dans TodoList.jsx (enfant)
export function TodoList(props) {
    const { todos, selectedTab } = props
    // Utilisation des props...
}
```

## 3. Le State (État)

Le state permet de gérer les données qui peuvent changer dans un composant.

### Exemple avec useState :
```jsx
// Dans App.jsx
const [todos, setTodos] = useState([
    { input: 'Hello! Add your first todo!', complete: true }
])
const [selectedTab, setSelectedTab] = useState('Open')
```

### Règles importantes du state :
1. Ne modifiez jamais le state directement
2. Utilisez toujours la fonction de mise à jour (setTodos, setSelectedTab)
3. Les mises à jour du state sont asynchrones

## 4. Les Hooks

### useState
```jsx
const [state, setState] = useState(initialValue)
```

### useEffect
```jsx
// Exemple de notre Todo App
useEffect(() => {
    if (!localStorage || !localStorage.getItem('todo-app')) { return }
    let db = JSON.parse(localStorage.getItem('todo-app'))
    setTodos(db.todos)
}, []) // Le tableau vide signifie que l'effet ne s'exécute qu'une fois
```

## 5. La Gestion des Événements

### Exemple de gestion d'événements dans notre app :
```jsx
// Dans Tabs.jsx
<button onClick={() => {
    setSelectedTab(tab)
}}>
    {tab}
</button>
```

## 6. Le Rendu Conditionnel

### Exemple avec les onglets :
```jsx
const filterTodosList = selectedTab === "All" ?
    todos :
    selectedTab === "Completed" ?
        todos.filter(val => val.complete) :
        todos.filter(val => !val.complete)
```

## 7. Les Listes et les Clés

### Exemple de rendu de liste :
```jsx
{filterTodosList.map((todo, todoIndex) => {
    return (
        <TodoCard
            key={todoIndex}
            {...props}
            todoIndex={todoIndex}
            todo={todo}
        />
    )
})}
```

## 8. Les Bonnes Pratiques

1. **Séparation des Responsabilités**
   - Chaque composant a une seule responsabilité
   - Exemple : TodoInput pour l'ajout, TodoList pour l'affichage

2. **Props Drilling**
   - Passage des props du parent vers les enfants
   - Dans notre cas : App.jsx → TodoList → TodoCard

3. **Gestion d'État**
   - État local pour les composants simples
   - État global (dans App.jsx) pour les données partagées

4. **Nommage**
   - Composants : PascalCase (TodoCard)
   - Props : camelCase (handleCompleteTodo)
   - Fonctions : camelCase (handleAddTodo)

## 9. Les Fonctionnalités Avancées Utilisées

1. **LocalStorage**
   ```jsx
   function handleSaveData(currTodos) {
       localStorage.setItem('todo-app', JSON.stringify({ todos: currTodos }))
   }
   ```

2. **Filtrage de Données**
   ```jsx
   todos.filter(val => !val.complete)
   ```

3. **Spread Operator**
   ```jsx
   const newTodoList = [...todos, { input: newTodo, complete: false }]
   ```

## 10. Points d'Attention

1. **Performance**
   - Utilisation de key dans les listes
   - Éviter les re-rendus inutiles

2. **Sécurité**
   - Validation des entrées utilisateur
   - Gestion des erreurs

3. **Maintenance**
   - Code modulaire
   - Commentaires explicatifs
   - Nommage clair

## 11. Ressources Utiles

- [Documentation React](https://reactjs.org/docs/getting-started.html)
- [React Hooks](https://reactjs.org/docs/hooks-intro.html)
- [React Patterns](https://reactpatterns.com/) 