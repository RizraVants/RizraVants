- 👋 Hi, I’m @RizraVants
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
RizraVants/RizraVants is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
/**
 * A simple mutex that can be used as a decorator. For examples, see Tests.Mutex.ts
 * @param keyGetter if you want to lock functions based on certain arguments, specify the key for the function based on the arguments
 */
export function Mutex (keyGetter?: (...args: any[]) => string) {
    let tasks: { [k: string]: Promise<void> } = {}
    return function (_, __, descriptor: PropertyDescriptor) {
        const originalMethod = descriptor.value
        descriptor.value = function (this: Object, ...args) {
            const key = (keyGetter && keyGetter.call(this, ...args)) || 'undefined'

            tasks[key] = (async () => {
                try {
                    tasks[key] && await tasks[key]
                } catch {

                }
                const result = await originalMethod.call(this, ...args)
                return result
            })()
            return tasks[key]
        }
    }
- 
