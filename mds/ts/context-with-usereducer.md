# Context API with useReducer

```tsx
import { createContext, useContext, useReducer, type ReactNode } from "react";

type Task = {
  task: string;
  isCompleted: boolean;
  id: number;
};

type Tasks = Task[];

type ActionAdd = {
  type: "ADD_TASK";
  payload: string;
};

type ActionDeleteComplete = {
  type: "DELETE_TASK" | "COMPLETE_TASK";
  payload: number;
};

type Action = ActionAdd | ActionDeleteComplete;

type TasksContextValue = {
  tasks: Tasks;
  addTask: (task: string) => void;
  deleteTask: (id: number) => void;
  completeTask: (id: number) => void;
};

type TasksProviderProps = {
  children: ReactNode;
};

const initialState: Tasks = [
  { task: "Learn TypeScript", isCompleted: false, id: 10 },
];

function tasksReducer(state: Tasks, action: Action): Tasks {
  const { type } = action;

  if (type === "ADD_TASK") {
    return [
      ...state,
      { task: action.payload, isCompleted: false, id: Math.random() },
    ];
  }

  if (type === "DELETE_TASK") {
    return state.filter((task) => task.id !== action.payload);
  }

  if (type === "COMPLETE_TASK") {
    return state.map((task) =>
      task.id === action.payload ? { ...task, isCompleted: true } : task
    );
  }

  return state;
}

const TasksContext = createContext<TasksContextValue | null>(null);

export default function TasksProvider({ children }: TasksProviderProps) {
  const [tasksState, dispatch] = useReducer(tasksReducer, initialState);

  const context: TasksContextValue = {
    tasks: tasksState,
    addTask(task) {
      dispatch({ type: "ADD_TASK", payload: task });
    },
    deleteTask(id) {
      dispatch({ type: "DELETE_TASK", payload: id });
    },
    completeTask(id) {
      dispatch({ type: "COMPLETE_TASK", payload: id });
    },
  };

  return (
    <TasksContext.Provider value={context}>{children}</TasksContext.Provider>
  );
}

export function useTasks() {
  const context = useContext(TasksContext);
  if (!context) throw new Error("TasksContext Error");
  return context;
}
```
