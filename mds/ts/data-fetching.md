# Data Fetching

```tsx
// export async function get(url: string) {
export async function get<T>(url: string) {
  const response = await fetch(url);

  if (!response.ok) throw new Error("Failed to fetch data.");

  const data = (await response.json()) as unknown;
  // return data;
  return data as T;
}
```

```tsx
import { type ReactNode, useEffect, useState } from "react";
import { get } from "./get.ts";

type Data = {
  id: number;
  title: string;
  text: string;
};

type RawData = {
  id: number;
  userId: number;
  title: string;
  text: string;
};

export default function App() {
  const [data, setData] = useState<Data[]>();
  const [isFetching, setIsFetching] = useState<boolean>(false);
  const [error, setError] = useState<string>();

  useEffect(() => {
    async function fetchData() {
      setIsFetching(true);

      try {
        // const data = ((await get) > "https://api-endpoint") as RawData[];
        const rawData = await get<RawData[]>("https://api-endpoint");

        const data: Data[] = rawData.map(({ id, title, text }) => {
          return { id, title, text };
        });

        setData(data);
      } catch (error) {
        // 1. setError("Failed to fetch posts!");
        // 2. setError((error as Error).message);
        // 3. if (error instanceof Error) setError(error.message);
        if (error instanceof Error) setError(error.message);
      }

      setIsFetching(false);
    }
    fetchData();
  }, []);

  let content: ReactNode;

  if (isFetching) {
    content = <p>Fetching...</p>;
  } else if (data) {
    content = <RenderData data={data} />;
  } else if (error) {
    content = <p>{error}</p>;
  }

  return <main>{content}</main>;
}
```
