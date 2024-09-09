## 运行

1. Install Dependencies
   ```bash
   npm i
   ```
2. Run Project

   ```bash
   npm run start
   ```

## 接入 v0

example：cra 创建基础模板+shadcn-UI+tailwindcss+axios

1. 初始化 ts 模板: `npx create-react-app shadcn-ui-cra-example --template typescript`

2. 安装依赖:
   ```
   	npm install tailwindcss-animate class-variance-authority clsx tailwind-merge lucide-react
   ```
3. 创建`tailwind.config.js` 并拷贝：

   ```javascript
   const { fontFamily } = require("tailwindcss/defaultTheme");

   /** @type {import('tailwindcss').Config} */
   module.exports = {
     darkMode: ["class"],
     content: ["./src/**/*.{js,jsx,ts,tsx}"],
     theme: {
       container: {
         center: true,
         padding: "2rem",
         screens: {
           "2xl": "1400px",
         },
       },
       extend: {
         colors: {
           border: "hsl(var(--border))",
           input: "hsl(var(--input))",
           ring: "hsl(var(--ring))",
           background: "hsl(var(--background))",
           foreground: "hsl(var(--foreground))",
           primary: {
             DEFAULT: "hsl(var(--primary))",
             foreground: "hsl(var(--primary-foreground))",
           },
           secondary: {
             DEFAULT: "hsl(var(--secondary))",
             foreground: "hsl(var(--secondary-foreground))",
           },
           destructive: {
             DEFAULT: "hsl(var(--destructive))",
             foreground: "hsl(var(--destructive-foreground))",
           },
           muted: {
             DEFAULT: "hsl(var(--muted))",
             foreground: "hsl(var(--muted-foreground))",
           },
           accent: {
             DEFAULT: "hsl(var(--accent))",
             foreground: "hsl(var(--accent-foreground))",
           },
           popover: {
             DEFAULT: "hsl(var(--popover))",
             foreground: "hsl(var(--popover-foreground))",
           },
           card: {
             DEFAULT: "hsl(var(--card))",
             foreground: "hsl(var(--card-foreground))",
           },
         },
         borderRadius: {
           lg: "var(--radius)",
           md: "calc(var(--radius) - 2px)",
           sm: "calc(var(--radius) - 4px)",
         },
         fontFamily: {
           sans: ["var(--font-sans)", ...fontFamily.sans],
         },
         keyframes: {
           "accordion-down": {
             from: { height: 0 },
             to: { height: "var(--radix-accordion-content-height)" },
           },
           "accordion-up": {
             from: { height: "var(--radix-accordion-content-height)" },
             to: { height: 0 },
           },
         },
         animation: {
           "accordion-down": "accordion-down 0.2s ease-out",
           "accordion-up": "accordion-up 0.2s ease-out",
         },
       },
     },
     plugins: [require("tailwindcss-animate")],
   };
   ```

4. 添加全局样式 `src/index.css`:

   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;

   @layer base {
     :root {
       --background: 0 0% 100%;
       --foreground: 222.2 47.4% 11.2%;

       --muted: 210 40% 96.1%;
       --muted-foreground: 215.4 16.3% 46.9%;

       --popover: 0 0% 100%;
       --popover-foreground: 222.2 47.4% 11.2%;

       --card: 0 0% 100%;
       --card-foreground: 222.2 47.4% 11.2%;

       --border: 214.3 31.8% 91.4%;
       --input: 214.3 31.8% 91.4%;

       --primary: 222.2 47.4% 11.2%;
       --primary-foreground: 210 40% 98%;

       --secondary: 210 40% 96.1%;
       --secondary-foreground: 222.2 47.4% 11.2%;

       --accent: 210 40% 96.1%;
       --accent-foreground: 222.2 47.4% 11.2%;

       --destructive: 0 100% 50%;
       --destructive-foreground: 210 40% 98%;

       --ring: 215 20.2% 65.1%;

       --radius: 0.5rem;
     }

     .dark {
       --background: 224 71% 4%;
       --foreground: 213 31% 91%;

       --muted: 223 47% 11%;
       --muted-foreground: 215.4 16.3% 56.9%;

       --popover: 224 71% 4%;
       --popover-foreground: 215 20.2% 65.1%;

       --card: 224 71% 4%;
       --card-foreground: 213 31% 91%;

       --border: 216 34% 17%;
       --input: 216 34% 17%;

       --primary: 210 40% 98%;
       --primary-foreground: 222.2 47.4% 1.2%;

       --secondary: 222.2 47.4% 11.2%;
       --secondary-foreground: 210 40% 98%;

       --accent: 216 34% 17%;
       --accent-foreground: 210 40% 98%;

       --destructive: 0 63% 31%;
       --destructive-foreground: 210 40% 98%;

       --ring: 216 34% 17%;

       --radius: 0.5rem;
     }
   }

   @layer base {
     * {
       @apply border-border;
     }
     body {
       @apply bg-background text-foreground;
       font-family: sans-serif;
     }
   }
   ```

5. 编辑 ts 配置 `tsconfig.json` :
   ```json
   {
     "compilerOptions": {
       "target": "es5",
       "lib": ["dom", "dom.iterable", "esnext"],
       "allowJs": true,
       "skipLibCheck": true,
       "esModuleInterop": true,
       "allowSyntheticDefaultImports": true,
       "strict": true,
       "forceConsistentCasingInFileNames": true,
       "noFallthroughCasesInSwitch": true,
       "module": "esnext",
       "moduleResolution": "node",
       "resolveJsonModule": true,
       "isolatedModules": true,
       "noEmit": true,
       "jsx": "react-jsx",
       // Add these Lines 👇
       "baseUrl": ".",
       "paths": {
         "@/*": ["./src/*"]
       }
     },
     "include": [
       "src",
       // Add TS Files 👇
       "**/*.ts",
       "**/*.tsx"
     ]
   }
   ```
6. 新建`src/lib/utils.ts`：

   ```typescript
   import { clsx, type ClassValue } from "clsx";
   import { twMerge } from "tailwind-merge";

   export function cn(...inputs: ClassValue[]) {
     return twMerge(clsx(inputs));
   }
   ```

7. 新增第一个 Shadcn/ui 组件. 运行: `npx shadcn-ui add button` and edit the path command to be `@/src/components/ui`

   [ts 中能够配置@路径别名简化路径处理](https://juejin.cn/post/7035580740850941989)

8. 删除`App.css` 文件并重写`App.tsx`:

   ```typescript
   import { useState } from "react";
   import { Button } from "@/components/ui/button";

   function App() {
     const [count, setCount] = useState(0);
     return (
       <div className="App">
         <header className="h-screen flex flex-col items-center justify-center">
           <p>
             Edit <code>src/App.tsx</code> and save to reload.
           </p>
           <Button asChild variant="link">
             <a
               href="https://reactjs.org"
               target="_blank"
               rel="noopener noreferrer"
             >
               Learn React
             </a>
           </Button>
           <Button
             variant="outline"
             onClick={() => setCount((count) => count + 1)}
           >
             Count is {count}
           </Button>
         </header>
       </div>
     );
   }

   export default App;
   ```

9. 启动:

```bash
npm run start
```
