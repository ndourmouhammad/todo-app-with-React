# Explication Détaillée du Todo App

## 1. Structure Générale de l'Application

L'application est composée de plusieurs composants React qui travaillent ensemble :

```
App.jsx (Composant Principal)
├── Header.jsx (En-tête)
├── Tabs.jsx (Filtres)
├── TodoList.jsx (Liste des tâches)
│   └── TodoCard.jsx (Carte individuelle)
└── TodoInput.jsx (Formulaire d'ajout)
```

## 2. Gestion des Données (App.jsx)

### État Principal
```jsx
const [todos, setTodos] = useState([
    { input: 'Hello! Add your first todo!', complete: true }
])
const [selectedTab, setSelectedTab] = useState('Open')
```
- `todos` : Tableau contenant toutes les tâches
- Chaque tâche est un objet avec deux propriétés :
  - `input` : Le texte de la tâche
  - `complete` : État de complétion (true/false)
- `selectedTab` : Gère l'onglet actif pour le filtrage

### Persistance des Données
```jsx
function handleSaveData(currTodos) {
    localStorage.setItem('todo-app', JSON.stringify({ todos: currTodos }))
}

useEffect(() => {
    if (!localStorage || !localStorage.getItem('todo-app')) { return }
    let db = JSON.parse(localStorage.getItem('todo-app'))
    setTodos(db.todos)
}, [])
```
- Sauvegarde automatique dans le localStorage
- Chargement des données au démarrage de l'application

## 3. Gestion des Tâches

### Ajout d'une Tâche
```jsx
function handleAddTodo(newTodo) {
    const newTodoList = [...todos, { input: newTodo, complete: false }]
    setTodos(newTodoList)
    handleSaveData(newTodoList)
}
```
- Crée une nouvelle tâche avec `complete: false`
- Utilise le spread operator pour préserver les tâches existantes
- Sauvegarde automatiquement dans le localStorage

### Suppression d'une Tâche
```jsx
function handleDeleteTodo(index) {
    let newTodoList = todos.filter((val, valIndex) => {
        return valIndex !== index
    })
    setTodos(newTodoList)
    handleSaveData(newTodoList)
}
```
- Filtre le tableau pour exclure la tâche à l'index spécifié
- Met à jour l'état et sauvegarde

### Complétion d'une Tâche
```jsx
function handleCompleteTodo(index) {
    const newTodoList = todos.map((todo, todoIndex) => {
        if (todoIndex === index) {
            return { ...todo, complete: !todo.complete }
        }
        return todo
    })
    setTodos(newTodoList)
    handleSaveData(newTodoList)
}
```
- Inverse l'état de complétion de la tâche
- Utilise map pour créer un nouveau tableau
- Préserve les autres propriétés avec le spread operator

## 4. Système de Filtrage (Tabs.jsx)

### Structure des Onglets
```jsx
const tabs = ["All", "Open", "Completed"]
```
- Trois catégories de filtrage
- Chaque onglet affiche le nombre de tâches correspondantes

### Logique de Filtrage
```jsx
const numOfTasks = tab === 'All' ?
    todos.length :
    tab === 'Open' ?
        todos.filter(val => !val.complete).length :
        todos.filter(val => val.complete).length
```
- "All" : Affiche toutes les tâches
- "Open" : Filtre les tâches non complétées
- "Completed" : Filtre les tâches complétées

## 5. Affichage des Tâches (TodoList.jsx)

### Filtrage Dynamique
```jsx
const filterTodosList = selectedTab === "All" ?
    todos :
    selectedTab === "Completed" ?
        todos.filter(val => val.complete) :
        todos.filter(val => !val.complete)
```
- Filtre les tâches en fonction de l'onglet sélectionné
- Met à jour automatiquement l'affichage

### Rendu des Tâches
```jsx
{filterTodosList.map((todo, todoIndex) => {
    const tempTodoIndex = todos.findIndex(val => val.input == todo.input)
    return (
        <TodoCard
            key={todoIndex}
            {...props}
            todoIndex={tempTodoIndex}
            todo={todo}
        />
    )
})}
```
- Utilise map pour créer une carte par tâche
- Trouve l'index correct dans le tableau original
- Passe toutes les props nécessaires à TodoCard

## 6. Flux de Données

1. **Entrée Utilisateur**
   - Ajout de tâche → TodoInput.jsx
   - Complétion/Suppression → TodoCard.jsx
   - Changement d'onglet → Tabs.jsx

2. **Mise à Jour de l'État**
   - Les actions utilisateur déclenchent des fonctions dans App.jsx
   - L'état est mis à jour via setTodos
   - Les données sont sauvegardées dans localStorage

3. **Re-rendu des Composants**
   - Les composants enfants reçoivent les nouvelles props
   - L'interface est mise à jour automatiquement

## 7. Points Forts de l'Architecture

1. **Séparation des Responsabilités**
   - Chaque composant a un rôle spécifique
   - Logique centralisée dans App.jsx
   - Composants enfants purement présentationnels

2. **Gestion d'État Efficace**
   - État centralisé
   - Mises à jour atomiques
   - Persistance automatique

3. **Interface Réactive**
   - Mise à jour instantanée
   - Filtrage dynamique
   - Expérience utilisateur fluide

## 8. Améliorations Possibles

1. **Performance**
   - Utiliser useMemo pour les calculs de filtrage
   - Implémenter la virtualisation pour les longues listes

2. **Fonctionnalités**
   - Ajouter l'édition des tâches
   - Implémenter la catégorisation
   - Ajouter des dates d'échéance

3. **UX/UI**
   - Ajouter des animations
   - Implémenter le drag-and-drop
   - Ajouter un mode sombre 