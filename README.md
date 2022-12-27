*`allowUnreachableCode` - `undefined` (по умолчанию) предоставляет предложения в качестве предупреждений редакторам
                         `true` недоступный код игнорируется
                         `false` вызывает ошибки компилятора о недоступном коде 

Эти предупреждения касаются только кода, который доказуемо недоступен из-за использования синтаксиса JavaScript

                         function fn(n: number) {
                          if (n > 5) {
                              return true;
                          } else {
                              return false;
                          }
                          return true;
                          }
                          С"allowUnreachableCode": false:
                          function fn(n: number) {
                          if (n > 5) {
                              return true;
                          } else {
                              return false;
                          }
                          return true;
                              | Unreachable code detected.
                          }

* `allowUnusedLabels`  * `undefined` (по умолчанию) предоставляет предложения в качестве предупреждений редакторам
 `true` неиспользуемые метки игнорируются
 `false` вызывает ошибки компилятора о неиспользуемых метках

                         function verifyAge(age: number) {
                           // Forgot 'return' statement
                           if (age > 18) {
                             verified: true;
                         | Unused label.
                           }
                         }  

*`alwaysStrict` - Гарантирует, что ваши файлы анализируются в строгом режиме ECMAScript и выдают “use strict” для каждого исходного файла.

*`exactOptionalPropertyTypes` - При включенном exactOptionalPropertyTypes TypeScript применяет более строгие правила в отношении того, как он обрабатывает свойства `type` или `interfaces`
которые имеют префикс `?`.
Например, этот интерфейс объявляет, что существует свойство, которое может быть одной из двух строк: ‘dark’ или ‘light’

                                   interface UserDefaults {
                                   // The absence of a value represents 'system'
                                   colorThemeOverride?: "dark" | "light";
                                 }

Если этот флаг не включен, вы можете установить три значения `colorThemeOverride:` `“темный”`, `“светлый”` и `undefined`.
Установка значения равным `undefined` позволит большинству проверок во время выполнения JavaScript завершиться неудачей, что фактически является ложным.
 `exactOptionalPropertyTypes` позволяет TypeScript действительно применять определение, предоставленное в качестве необязательного свойства:


                                 const settings = getUserSettings();
                                 settings.colorThemeOverride = "dark";
                                 settings.colorThemeOverride = "light";
 
                                 // But not:
                                 settings.colorThemeOverride = undefined;
                                 | Type 'undefined' is not assignable to type '"dark" | "light"' with 
                                 | 'exactOptionalPropertyTypes: true'. Consider adding 'undefined' to the 
                                 | type of the target.

*`noFallthroughCasesInSwitch` - Гарантирует, что любой непустой регистр внутри оператора `switch` включает либо `break` или `return`. Это означает, что вы случайно не отправите ошибку в случаев
сбоя.  

                                const a: number = 6;
 
                                switch (a) {
                                case 0:
                                | Fallthrough case in switch.
                                    console.log("even");
                                case 1:
                                    console.log("odd");
                                    break;
                                }                               

*`noImplicitAny` - В некоторых случаях, когда аннотации типов отсутствуют, TypeScript возвращается к типу `any`для переменной, когда он не может определить тип.
Однако включение TypeScript будет выдавать ошибку всякий раз, когда она будет выведена `any`:

                   function fn(s) {
                    | Parameter 's' implicitly has an 'any' type.
                      console.log(s.subtr(3));
                    }

*`noImplicitOverride` - При работе с классами, использующими наследование, подкласс может “рассинхронизироваться” с функциями, которые он перегружает, когда они переименовываются в базовом классе.

                      class Album {
                         download() {
                           // Default behavior
                         }
                       }
 
                       class SharedAlbum extends Album {
                         download() {
                           // Override to get info from many sources
                         }
                       }

Затем, когда вы добавляете поддержку списков воспроизведения, созданных с помощью машинного обучения, вы реорганизуете класс `Album`, чтобы вместо него была функция `setup`:

                       class Album {
                          setup() {
                            // Default behavior
                          }
                        }
 
                        class MLAlbum extends Album {
                          setup() {
                            // Override to get info from algorithm
                          }
                        }
 
                        class SharedAlbum extends Album {
                          download() {
                            // Override to get info from many sources
                          }
                        }

В этом случае TypeScript не предоставил никакого предупреждения о том, что `download` `SharedAlbum` ожидается переопределение функции в базовом классе.
Используя `noImplicitOverride` вы можете гарантировать, что подклассы никогда не будут синхронизированы, гарантируя, что переопределяющие функции включают ключевое слово `override`.
Следующий пример `noImplicitOverride` включен

                        class Album {
                           setup() {}
}
 
                         class MLAlbum extends Album {
                           override setup() {}
                         }
 
                         class SharedAlbum extends Album {
                           setup() {}

                         | This member must have an 'override' modifier because it overrides a member in the base class 'Album'.
                         }

*`noImplicitReturns` - При включении TypeScript проверяет все пути кода в функции, чтобы убедиться, что они возвращают значение.

                        function lookupHeadphonesManufacturer(color: "blue" | "black"): string {

                        | Function lacks ending return statement and return type does not include 'undefined'.

                          if (color === "blue") {
                            return "beats";
                          } else {
                            "bose";
                          }
                        }

*`noImplicitThis` - Выдает ошибку в выражениях ‘this’ с подразумеваемым типом ‘any’.

                    class Rectangle {
                      width: number;
                      height: number;
                     
                      constructor(width: number, height: number) {
                        this.width = width;
                        this.height = height;
                      }
                     
                      getAreaFunction() {
                        return function () {
                          return this.width * this.height;
                   | 'this' implicitly has type 'any' because it does not have a type annotation.
                   | 'this' implicitly has type 'any' because it does not have a type annotation.
                        };
                      }
                    }

*`noPropertyAccessFromIndexSignature` - Этот параметр обеспечивает согласованность между доступом к полю с помощью синтаксиса “точка” (`obj.key`) и “индексированный” (`obj["key"]`) и 
способом объявления свойства в типе. Без этого флага TypeScript позволит вам использовать синтаксис dot для доступа к полям, которые не определены:

                                        interface GameSettings {
                                          // Known up-front properties
                                          speed: "fast" | "medium" | "slow";
                                          quality: "high" | "low";
                                         
                                          // Assume anything unknown to the interface
                                          // is a string.
                                          [key: string]: string;
                                        }
                                         
                                        const settings = getSettings();
                                        settings.speed;
                                        // (property) GameSettings.speed: "fast" | "medium" | "slow"
                                                  

                                        settings.quality;
                                        // (property) GameSettings.quality: "high" | "low"
                                                   
                                         
                                        // Unknown key accessors are allowed on
                                        // this object, and are `string`
                                        settings.username;
                                        // String

Включение флага приведет к появлению ошибки, поскольку неизвестное поле использует синтаксис точки вместо индексированного синтаксиса.

                                        const settings = getSettings();
                                         settings.speed;
                                         settings.quality;
                                          
                                         // This would need to be settings["username"];
                                         settings.username;
                                         | Property 'username' comes from an index signature, so it must be accessed with ['username'].

*`noUncheckedIndexedAccess` - В TypeScript есть способ описания объектов, которые имеют неизвестные ключи, но известные значения для объекта, с помощью индексных подписей.
Включение `noUncheckedIndexedAccess` добавит `undefined` к любому не объявленному полю в типе.

                              declare const env: EnvironmentVars;
 
                                // Declared as existing
                                const sysName = env.NAME;
                                const os = env.OS;
                                // const os: string
                                      
                                 
                                // Not declared, but because of the index
                                // signature, then it is considered a string
                                const nodeEnv = env.NODE_ENV;
                                // const nodeEnv: string | undefined

*`noUnusedLocals` - Сообщать об ошибках в неиспользуемых локальных переменных.

                   const createKeyboard = (modelID: number) => {
                      const defaultModelID = 23;
                      | 'defaultModelID' is declared but its value is never read.
                      return { type: "keyboard", modelID };
                    };   

*`noUnusedParameters` - Сообщать об ошибках по неиспользуемым параметрам в функциях.

                        const createDefaultKeyboard = (modelID: number) => {
                        | 'modelID' is declared but its value is never read.
                          const defaultModelID = 23;
                          return { type: "keyboard", modelID: defaultModelID };
                        };

*`strict` - Флаг обеспечивает широкий диапазон действий по проверке типов, что приводит к более надежным гарантиям корректности программы. 
Включение этого параметра равносильно включению всех параметров семейства строгих режимов, которые описаны ниже. 
Затем вы можете отключить отдельные проверки семейства строгих режимов по мере необходимости.

*`strictBindCallApply` - При установке TypeScript проверяет, что встроенные методы функций `call`, `bind`, и `apply` вызываются с правильным аргументом для базовой функции:

                         // With strictBindCallApply on
                         function fn(x: string) {
                           return parseInt(x);
                         }
                          
                         const n1 = fn.call(undefined, "10");
                          
                         const n2 = fn.call(undefined, false);
                         | Argument of type 'boolean' is not assignable to parameter of type 'string'.

*`strictFunctionTypes` - Когда этот флаг включен, параметры функций проверяются более корректно.

                        function fn(x: string) {
                          console.log("Hello, " + x.toLowerCase());
                        }
                         
                        type StringOrNumberFunc = (ns: string | number) => void;
                         
                        // Unsafe assignment is prevented
                        let func: StringOrNumberFunc = fn;
                        | Type '(x: string) => void' is not assignable to type 'StringOrNumberFunc'.
                        |   Types of parameters 'x' and 'ns' are incompatible.
                        |     Type 'string | number' is not assignable to type 'string'.
                        |       Type 'number' is not assignable to type 'string'.

*`strictNullChecks` - Когда `strictNullChecks` `false`, `null` и `undefined` фактически игнорируются языком. Это может привести к непредвиденным ошибкам во время выполнения.
Когда `strictNullChecks` `true`, `null` и `undefined` имеют свои собственные различные типы, и вы получите ошибку типа, если попытаетесь использовать их там, где ожидается конкретное значение.

                      declare const loggedInUsername: string;
 
                         const users = [
                           { name: "Oby", age: 12 },
                           { name: "Heera", age: 32 },
                         ];
                          
                         const loggedInUser = users.find((u) => u.name === loggedInUsername);
                         console.log(loggedInUser.age);

Установка `strictNullChecks` значения в trueвызовет сообщение об ошибке

                         declare const loggedInUsername: string;
 
                         const users = [
                           { name: "Oby", age: 12 },
                           { name: "Heera", age: 32 },
                         ];
                          
                         const loggedInUser = users.find((u) => u.name === loggedInUsername);
                         console.log(loggedInUser.age);
                         | Object is possibly 'undefined'.

*`strictPropertyInitialization` - Если установлено значение true, TypeScript выдаст сообщение об ошибке, если свойство класса было объявлено, но не задано в конструкторе.

                                  class UserAccount {
                                    name: string;
                                    accountType = "user";
                                   
                                    email: string;

                                  | Property 'email' has no initializer and is not definitely assigned in the constructor.

                                    address: string | undefined;
                                   
                                    constructor(name: string) {
                                      this.name = name;
                                      // Note that this.email is not set
                                    }
                                  }

В приведенном выше случае:

                                  `this.name` устанавливается специально.
                                  `this.accountType` устанавливается по умолчанию.
                                  `this.email` не задан и выдает ошибку.
                                  `this.address` объявляется как потенциально `undefined`, что означает, что его не нужно устанавливать.

*`useUnknownInCatchVariables` - В TypeScript 4.0 была добавлена поддержка, позволяющая изменять тип переменной в предложении `catch` с `any` на `unknown`.

                                try {
                                  // ...
                                } catch (err) {
                                  // We have to verify err is an
                                  // error before using it as one.
                                  if (err instanceof Error) {
                                    console.log(err.message);
                                  }
                                }

Этот шаблон гарантирует, что код обработки ошибок становится более полным, поскольку вы не можете гарантировать, 
что создаваемый объект является подклассом ошибок заранее. 
Если флаг `useUnknownInCatchVariables` включен, вам не нужны ни дополнительный синтаксис (`: unknown`), ни правило компоновки, чтобы попытаться применить это поведение.

                        

# Interop Constraint

*`allowSyntheticDefaultImports` - Если установлено значение true, allowSyntheticDefaultImportsпозволяет записывать импорт, например:

                                  import React from "react";
                                  
вместо:
                                  
                                  import * as React from "react";

Когда модуль явно не указывает экспорт по умолчанию.
Например, без `allowSyntheticDefaultImports` значения `true`:
                                  
                                  // @filename: utilFunctions.js

                                  | Module '"/home/runner/work/TypeScript-Website/TypeScript-Website/utilFunctions"' has no default export.

                                  const getStringLength = (str) => str.length;
                                   
                                  module.exports = {
                                    getStringLength,
                                  };
                                   
                                  // @filename: index.ts
                                  import utils from "./utilFunctions";
                                   
                                  const count = utils.getStringLength("Check JS");

Этот код выдает ошибку, поскольку нет defaultобъекта, который вы могли бы импортировать. 
Даже несмотря на то, что кажется, что так и должно быть. Для удобства транспайлеры, такие как Babel, 
автоматически создадут значение по умолчанию, если оно не создано. Сделать модуль более похожим на:

                                  // @filename: utilFunctions.js
                                  const getStringLength = (str) => str.length;
                                  const allFunctions = {
                                    getStringLength,
                                  };
                                  module.exports = allFunctions;
                                  module.exports.default = allFunctions;

Этот флаг не влияет на JavaScript, создаваемый TypeScript, 
он используется только для проверки типа. Этот параметр приводит поведение TypeScript в соответствие с Babel, 
где генерируется дополнительный код, чтобы сделать использование экспорта модуля по умолчанию более эргономичным.

`esModuleInterop` - По умолчанию (с `esModuleInterop` значением `false` или не установлено)
есть две части, которые оказались ошибочными предположениями:

* подобный импорт пространства `import * as moment from "moment"` имен действует так же, как `const moment = require("moment")`

* подобный импорт по умолчанию `import moment from "moment"` действует так же, как `const moment = require("moment").default`
                     
Это несоответствие вызывает эти две проблемы:
                     
* в спецификации модулей ES6 указано, что import (ёimport * as xё) пространства имен может быть только объектом, 
поскольку TypeScript обрабатывает его так же, как ё= require("x")ётогда TypeScript  разрешил, 
чтобы импорт обрабатывался как функция и был вызываемым. Это недопустимо в соответствии со спецификацией.
                     
* несмотря на точность спецификации модулей ES6, большинство библиотек с модулями CommonJS / AMD / UMD не соответствуют так строго, как реализация TypeScript.

Включение `esModuleInteropисправит` обе эти проблемы в коде, переданном с помощью TypeScript.


                     import * as fs from "fs";
                     import _ from "lodash";
                     fs.readFileSync("file.txt", "utf8");
                     _.chunk(["a", "b", "c", "d"], 2);

С `esModuleInterop` отключенным:

                     "use strict";
                      Object.defineProperty(exports, "__esModule", { value: true });
                      const fs = require("fs");
                      const lodash_1 = require("lodash");
                      fs.readFileSync("file.txt", "utf8");
                      lodash_1.default.chunk(["a", "b", "c", "d"], 2);

С `esModuleInterop` установленным значением `true`:

                      "use strict";
                       var __createBinding = (this && this.__createBinding) || (Object.create ? (function(o, m, k, k2) {
                           if (k2 === undefined) k2 = k;
                           var desc = Object.getOwnPropertyDescriptor(m, k);
                           if (!desc || ("get" in desc ? !m.__esModule : desc.writable || desc.configurable)) {
                             desc = { enumerable: true, get: function() { return m[k]; } };
                           }
                           Object.defineProperty(o, k2, desc);
                       }) : (function(o, m, k, k2) {
                           if (k2 === undefined) k2 = k;
                           o[k2] = m[k];
                       }));
                       var __setModuleDefault = (this && this.__setModuleDefault) || (Object.create ? (function(o, v) {
                           Object.defineProperty(o, "default", { enumerable: true, value: v });
                       }) : function(o, v) {
                           o["default"] = v;
                       });
                       var __importStar = (this && this.__importStar) || function (mod) {
                           if (mod && mod.__esModule) return mod;
                           var result = {};
                           if (mod != null) for (var k in mod) if (k !== "default" && Object.prototype.hasOwnProperty.call(mod, k)) __createBinding(result, mod, k);
                           __setModuleDefault(result, mod);
                           return result;
                       };
                       var __importDefault = (this && this.__importDefault) || function (mod) {
                           return (mod && mod.__esModule) ? mod : { "default": mod };
                       };
                       Object.defineProperty(exports, "__esModule", { value: true });
                       const fs = __importStar(require("fs"));
                       const lodash_1 = __importDefault(require("lodash"));
                       fs.readFileSync("file.txt", "utf8");
                       lodash_1.default.chunk(["a", "b", "c", "d"], 2);

*`isolatedModules` - Хотя вы можете использовать TypeScript для создания кода JavaScript из кода TypeScript, 
для этого также часто используются другие транспайлеры, такие как Babel. Однако другие транспайлеры одновременно 
работают только с одним файлом, что означает, что они не могут применять преобразования кода, которые зависят от 
понимания полной системы типов. Это ограничение также распространяется на `ts.transpileModule` API TypeScript, который 
используется некоторыми инструментами сборки.

Эти ограничения могут вызвать проблемы во время выполнения с некоторыми функциями TypeScript, такими как `const enums` и `namespaces`. 
Установка `isolatedModules` флага позволяет TypeScript предупреждать вас, если вы пишете определенный код, который не может быть 
правильно интерпретирован процессом переноса одного файла.

Это не изменяет поведение вашего кода или иным образом не изменяет поведение процесса проверки и отправки TypeScript.

Некоторые примеры кода, который не работает при `isolatedModules` 

### Экспорт идентификаторов, не имеющих значения

В TypeScript вы можете импортировать тип, а затем экспортировать его: 


                     `import { someType, someFunction } from "someModule";
 
                     someFunction();
                      
                     export { someType, someFunction };`


Поскольку для него нет значенияsomeType, отправленный exportфайл не будет пытаться экспортировать его (это было бы ошибкой времени выполнения в JavaScript):

                     `export { someFunction };`

Однофайловые транспайлеры не знают `someType`, выдает ли значение или нет, поэтому экспортировать имя, которое ссылается только на тип, является ошибкой.

### Немодульные файлы

Если `isolatedModules` задано, все файлы реализации должны быть модулями (что означает, что он имеет некоторую форму `import`/`export`). 
Если какой-либо файл не является модулем, возникает ошибка:
                     
                     function fn() {}
                     | 'index.ts' cannot be compiled under '--isolatedModules' because it is considered a 
                     | global script file. Add an import, export, or an empty 'export {}' statement to make it a module.

                     Это ограничение не распространяется на `.d.ts` файлы.

Ссылки на `const enum` участников
В TypeScript, когда вы ссылаетесь на `const enum`, ссылка заменяется его фактическим значением в исходящем JavaScript. Изменение этого TypeScript:
                     
                     `declare const enum Numbers {
                       Zero = 0,
                       One = 1,
                     }
                     console.log(Numbers.Zero + Numbers.One);`

К этому JavaScript:
                     
                     "use strict";
                     console.log(0 + 1);
                      
Без знания значений этих элементов другие транспиляторы не смогут заменить ссылки на `Numbers`, 
что было бы ошибкой во время выполнения, если оставить его в покое (поскольку `Numbers` во время выполнения нет объекта). 
Из-за этого, когда `isolatedModules` установлено значение, ссылка на окружающий `const enum` является ошибкой.
