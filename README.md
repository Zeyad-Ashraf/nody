const http = require('http');
const path = require('path');
const fs = require('fs');
const EventEmitter = require('events');
const os = require('os');

// 1- Write an HTTP server that handles file paths. Implement the following:
// • Task 1.1: Respond with a breakdown of a given file path (e.g., extract root,
// directory, file name, and extension) and return the full path in a formatted string.
// o URL: POST /path-info (Get the file path from the body)
// o Input: Provide a file path, such as C:/Users/example/project/sample.txt.

// http.createServer((req, res, next) => {
//     const { url, method } = req;
//     if (method === "POST" && url === "/path-info") {
//         let body = '';
//         req.on("data", (chunk) => {
//             body += JSON.parse(chunk);
//         }).on("end", () => {
//             if (body) {
//                 const response = {
//                     parsedPath: {
//                         root: path.parse(body).root,
//                         dir: path.dirname(body),
//                         base: path.basename(body),
//                         ext: path.extname(body),
//                         name: path.basename(body, path.extname(body))
//                     },
//                     formattedPath: body
//                 };
//                 res.write(JSON.stringify(response));
//                 res.end();
//             } else {
//                 res.write(JSON.stringify({ error: "No file path provided" }));
//                 res.end();
//             }
//         })
//     } else if (method === "POST" && url === "/path-check") {
//         let body = '';
//         req.on("data", (chunk) => {
//             body += JSON.parse(chunk);
//         }).on("end", () => {
//             if (body) {
//                 const response = {
//                     isAbsolute: path.isAbsolute(body),
//                     basename: path.basename(body),
//                     extname: path.extname(body),
//                     joinedPath: path.join(path.dirname(body), path.basename(body)),
//                     resolvedPath: path.resolve(body)
//                 };
//                 res.write(JSON.stringify(response));
//                 res.end();
//             } else {
//                 res.write(JSON.stringify({ error: "No file path provided" }));
//                 res.end();
//             }
//         })
//     } else {
//         res.write(JSON.stringify({ error: "Invalid request method" }));
//         res.end();
//     }
// }).listen(3000,()=>console.log('Server is listening on port 3000'));

// 2. Events Module (2 Grades)
// Create an event emitter to handle file operations. Implement the following:
// • Task 2: Emit an event when a file is created, read, or deleted.
// o URLs:
// ▪ POST /create-file (Get the file name from the body)
// ▪ DELETE /delete-file (Get the file name from the body)
// o Input Example for File Creation:
// o Expected Output (Event Logged to the console):
// “Event emitted: fileCreated for example.txt”

// function createFile(fileName) {
//     const filePath = `./${fileName}`;
//     fs.writeFile(filePath, "Hello World!", (err) => {
//         (err) ? console.log(err) : console.log(`File ${fileName} created`);
//     });
// }
// function deleteFile(fileName) {
//     const filePath = `./${fileName}`;
//     fs.unlink(filePath, (err) => {
//         (err) ? console.log(err) : console.log(`File ${fileName} deleted`);
//     });
// }
// http.createServer((req, res, next) => {
//     const { url, method } = req;
//     if (method === "POST" && url === "/create-file") {
//         let body = '';
//         req.on("data", (chunk) => {
//             body +=JSON.parse(chunk);
//         }).on("end", () => {
//             if (body) {
//                 const eventEmitter = new EventEmitter();
//                 const fileName = body.fileName;
//                 eventEmitter.on("fileCreated",()=> createFile(fileName)).emit("fileCreated");
//                 res.end("File created");
//             } else {
//                 res.end("No file to create");
//             }
//         })
//     } else if (method ==="DELETE"&& url==="/delete-file") {
//         let body = "";
//         req.on("data", (data) => {
//             body += JSON.parse(data);
//         }).on("end", () => {
//             if (body) {
//                 const eventEmitter = new EventEmitter();
//                 const fileName = body.fileName;
//                 eventEmitter.on("fileDeleted", ()=> deleteFile(fileName)).emit("fileDeleted");
//                 res.end("File deleted");
//             } else {
//                 res.end("No file to delete");
//             }
//         })
//     } else {
//         console.log("Invalid request method");
//     }
// }).listen(3000,()=>console.log('Server is listening on port 3000'));

// 3. OS Module (1 Grades) (Search Point)
// Gather system information and return it in the response.
// • Task: Respond with the system’s architecture, platform, free memory, and total
// memory.
// o URL: GET /system-info
// o Expected Output:

// http.createServer((req, res, next) => {
//     const { url, method } = req;
//     if (method === "GET" && url === "/system-info") {
//         let response = {
//             architecture: os.arch(),
//             platform: os.platform(),
//             freemem: os.freemem(),
//             totalmem: os.totalmem()
//         };
//         res.write(JSON.stringify(response));
//         res.end();
//     } else {
//         res.write(JSON.stringify({ error: "Invalid request method" }));
//         res.end();
//     }
// }).listen(3000, () => console.log('Server is listening on port 3000'));

// 4. File System Module (2 Grades)
// Perform file-based CRUD operations using a file to store data.
// o Task 3.1: Create and delete a file.
// • URLs:
// o POST /create-file (Get the file name from the body)
// o DELETE /delete-file (Get the file name from the body)
// • Input Example for File Creation:

// • Expected Output: The file is created, read, or deleted, and the relevant content
// or confirmation message is sent back.
// • Example (After creating the file):

// http.createServer((req, res, next) => {
//     const { url, method } = req;
//     if (method === "POST" && url === "/create-file") {
//         let body = '';
//         req.on("data", (chunk) => {
//             body += JSON.parse(chunk);
//         }).on("end", () => {
//             if (body) {
//                 const fileName = `./${body.fileName}`;
//                 fs.writeFile(fileName, "File created", (err) => {
//                     (err) ? res.write(JSON.stringify({ error: err.message })) : res.write(JSON.stringify({ message: "File created" }));
//                     res.end();
//                 });
//                 // res.write(JSON.stringify({ message: "File created" }));
//                 // res.end();
//             } else {
//                 res.write(JSON.stringify({ message: "No file name provided" }));
//                 res.end();
//             }
//         })
//     }else if (method === "DELETE" && url === "/delete-file") {
//         let body = '';
//         req.on("data", (chunk) => {
//             body += JSON.parse(chunk);
//         }).on("end", () => {
//             if (body) {
//                 fs.unlink(body.fileName, () => {
//                     res.write(JSON.stringify({ message: "File deleted" }));
//                     res.end();
//                 })
//             }
//             })
//     }else {
//         res.write(JSON.stringify({ error: "Invalid request method" }));
//         res.end();
//     }
// }).listen(3000,()=>console.log('Server is listening on port 3000'));

// Task 3.2: Asynchronously read from and append to a file. Use path.join() to create
// the file path and path.resolve() to get the absolute path before performing the read
// and write operations.
// • URLs:
// o POST /append-async (Get the file name from the body)
// o POST /read-async (Get the file name from the body)
// • Input Example for Asynchronous File Write:

// • Expected Output: The file is written or read asynchronously, with a
// confirmation message

// http.createServer((req, res, next) => {
//     const { url, method } = req;
//     if (method === "POST" && url === "/append-async") {
//         let body = '';
//         req.on("data", (chunk) => {
//             body = JSON.parse(chunk);
//         }).on("end", () => {
//             if (body) {
//                 const fileName = body.fileName;
//                 const filePath = path.join(__dirname, fileName);
//                 fs.appendFile(filePath, "Hello World!", (err) => {
//                     (err) ? res.write(JSON.stringify({ error: "error message "})) : res.write(JSON.stringify({ message: "File appended", fileName }));
//                     res.end();
//                 });
//             } else {
//                 res.write(JSON.stringify({ error: "No file to append" }));
//                 res.end();
//             }
//         });
//     } else if (method === "POST" && url === "/read-async") {
//         let body = '';
//         req.on("data", (chunk) => {
//             body = JSON.parse(chunk);
//         }).on("end", () => {
//             if (body) {
//                 const fileName = body.fileName;
//                 const resolvedPath = path.resolve(__dirname,fileName);
//                 fs.readFile(resolvedPath, 'utf8', (err, data) => {
//                     if (err) {
//                         res.write(JSON.stringify({ error: "error message" }));
//                         res.end();
//                     } else {
//                         res.write(JSON.stringify({ message:"The file is written or read asynchronously, with aconfirmation message"}));
//                         res.end();
//                     }
//                 });
//             }
//         })
//     } else {
//         res.write(JSON.write(JSON.stringify({ message: "No file specified to read" })));
//         res.end();
//     }
// }).listen(3000, () => console.log('Server is listening on port 3000'));
