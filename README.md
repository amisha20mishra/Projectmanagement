# Projectmanagement
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project Management System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0d0d0d;
            color: #e0e0e0;
            overflow-x: hidden;
            position: relative;
        }
        .animated-background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            background-image: radial-gradient(circle, rgba(74, 144, 226, 0.1) 1px, transparent 1px);
            background-size: 20px 20px;
            animation: move-dots 60s linear infinite;
        }
        .animated-background::before, .animated-background::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: radial-gradient(circle, rgba(74, 144, 226, 0.05) 1px, transparent 1px);
            background-size: 30px 30px;
            animation: move-dots 90s linear infinite reverse;
        }
        .animated-background::after {
            background-image: radial-gradient(circle, rgba(74, 144, 226, 0.15) 1px, transparent 1px);
            background-size: 40px 40px;
            animation: move-dots 120s linear infinite;
        }
        @keyframes move-dots {
            from {
                background-position: 0 0;
            }
            to {
                background-position: 100% 100%;
            }
        }
        .container {
            max-width: 900px;
        }
        .card {
            background-color: #1e1e1e;
            border-radius: 12px;
            box-shadow: 0 0 10px rgba(74, 144, 226, 0.3);
            transition: box-shadow 0.3s;
        }
        .card:hover {
            box-shadow: 0 0 15px rgba(74, 144, 226, 0.6);
        }
        .input-field {
            background-color: #2a2a2a;
            color: #f5f5f5;
            border: 1px solid #3a3a3a;
            transition: border-color 0.3s;
            box-shadow: 0 0 5px rgba(74, 144, 226, 0.2);
        }
        .input-field:focus {
            outline: none;
            border-color: #4a90e2;
            box-shadow: 0 0 8px rgba(74, 144, 226, 0.5);
        }
        .btn {
            transition: transform 0.2s ease-in-out, box-shadow 0.3s;
            box-shadow: 0 0 8px rgba(74, 144, 226, 0.4);
            animation: pulse-glow 2s infinite;
        }
        .btn:hover {
            transform: scale(1.02);
            box-shadow: 0 0 15px rgba(74, 144, 226, 0.7);
        }
        .task-item {
            display: flex;
            align-items: center;
            padding: 8px 0;
            transition: transform 0.2s ease;
        }
        .task-checkbox {
            margin-right: 12px;
            cursor: pointer;
            min-width: 20px;
            height: 20px;
            appearance: none;
            border: 2px solid #5a5a5a;
            border-radius: 4px;
            position: relative;
            transition: background-color 0.2s, border-color 0.2s;
        }
        .task-checkbox:checked {
            background-color: #4a90e2;
            border-color: #4a90e2;
        }
        .task-checkbox:checked::after {
            content: '✓';
            color: #fff;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 14px;
        }
        .icon-btn {
            opacity: 0.6;
            transition: opacity 0.2s;
        }
        .icon-btn:hover {
            opacity: 1;
        }
        .message-box {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            padding: 12px 24px;
            border-radius: 8px;
            font-weight: bold;
            z-index: 1000;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
            opacity: 0;
            transition: opacity 0.5s ease-in-out;
        }
        .message-box.show {
            opacity: 1;
        }
        .message-box.error {
            background-color: #b91c1c;
            color: white;
        }
        .message-box.success {
            background-color: #16a34a;
            color: white;
        }
        h1 {
            text-shadow: 0 0 10px rgba(74, 144, 226, 0.8);
        }
        @keyframes pulse-glow {
            0% { box-shadow: 0 0 8px rgba(74, 144, 226, 0.4); }
            50% { box-shadow: 0 0 12px rgba(74, 144, 226, 0.8); }
            100% { box-shadow: 0 0 8px rgba(74, 144, 226, 0.4); }
        }
    </style>
</head>
<body class="p-4">
    <div class="animated-background"></div>
    <div class="container mx-auto relative z-10">
        <header class="text-center my-8">
            <h1 class="text-4xl font-bold text-gray-50">Project Manager</h1>
            <p class="text-gray-400 mt-2">Simplify your workflow and get organized.</p>
        </header>

        <!-- User Information -->
        <div class="text-sm text-center mb-6">
            <p>User ID: <span id="userIdDisplay" class="font-mono text-xs text-gray-400">Loading...</span></p>
        </div>

        <!-- Add New Project Section -->
        <div class="card p-6 mb-8">
            <h2 class="text-2xl font-semibold mb-4 text-gray-50">Create a New Project</h2>
            <div class="flex flex-col sm:flex-row space-y-4 sm:space-y-0 sm:space-x-4">
                <input type="text" id="newProjectName" class="input-field w-full p-3 rounded-lg" placeholder="Enter project name..." required>
                <button id="addProjectBtn" class="btn bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg shadow-lg">Add Project</button>
            </div>
        </div>

        <!-- Search Section -->
        <div class="card p-6 mb-8">
            <h2 class="text-2xl font-semibold mb-4 text-gray-50">Search Projects</h2>
            <input type="text" id="searchInput" class="input-field w-full p-3 rounded-lg" placeholder="Search by project or task name..." required>
        </div>

        <!-- Projects List Section -->
        <div id="projectsContainer" class="space-y-6">
            <div class="text-center text-gray-400 mt-10" id="loadingMessage">Loading projects...</div>
        </div>
    </div>

    <!-- Message Box for user notifications -->
    <div id="messageBox" class="message-box"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, doc, updateDoc, onSnapshot, query, getDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Global variables provided by the Canvas environment
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);

        // UI Elements
        const projectsContainer = document.getElementById('projectsContainer');
        const newProjectNameInput = document.getElementById('newProjectName');
        const addProjectBtn = document.getElementById('addProjectBtn');
        const userIdDisplay = document.getElementById('userIdDisplay');
        const loadingMessage = document.getElementById('loadingMessage');
        const messageBox = document.getElementById('messageBox');
        const searchInput = document.getElementById('searchInput');

        let userId = null;
        let allProjects = [];

        // Utility function to show a message to the user
        const showMessage = (message, type) => {
            messageBox.textContent = message;
            messageBox.className = `message-box show ${type}`;
            setTimeout(() => {
                messageBox.classList.remove('show');
            }, 3000);
        };

        // LLM Integration
        const generateTasksWithLLM = async (projectName, projectId) => {
            try {
                const prompt = `Given a project named "${projectName}", break it down into a list of 5-7 detailed, actionable tasks. Respond as a JSON array of strings, where each string is a task description. Do not include any other text or formatting. Example: ["Task 1", "Task 2"].`;

                const payload = {
                    contents: [{ parts: [{ text: prompt }] }],
                    generationConfig: {
                        responseMimeType: "application/json",
                        responseSchema: {
                            type: "ARRAY",
                            items: {
                                type: "STRING"
                            }
                        }
                    }
                };
                
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                
                if (!response.ok) {
                    throw new Error(`API returned status ${response.status}`);
                }
                
                const result = await response.json();
                const json = result.candidates?.[0]?.content?.parts?.[0]?.text;
                const tasks = JSON.parse(json);

                if (Array.isArray(tasks)) {
                    const projectsRef = getProjectsCollectionRef();
                    const projectDocRef = doc(projectsRef, projectId);
                    const docSnap = await getDoc(projectDocRef);
                    
                    if (docSnap.exists()) {
                        const currentTasks = docSnap.data().tasks || [];
                        const newTasks = tasks.map(desc => ({ description: desc, completed: false, createdAt: new Date() }));
                        await updateDoc(projectDocRef, {
                            tasks: [...currentTasks, ...newTasks]
                        });
                        showMessage("Tasks generated and added successfully!", "success");
                    }
                } else {
                    throw new Error("Invalid response format from LLM.");
                }

            } catch (error) {
                console.error("Error generating tasks with LLM: ", error);
                showMessage("Failed to generate tasks. Try a different project name.", "error");
            }
        };

        const summarizeProjectWithLLM = async (project) => {
            try {
                const taskList = project.tasks.map(task => 
                    `[${task.completed ? 'COMPLETED' : 'INCOMPLETE'}] ${task.description}`
                ).join('\n');

                const prompt = `Based on the following project and its tasks, provide a concise, single-paragraph summary of its status and progress.
Project Name: ${project.name}
Tasks:
${taskList}`;

                const payload = {
                    contents: [{ parts: [{ text: prompt }] }],
                };
                
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                
                if (!response.ok) {
                    throw new Error(`API returned status ${response.status}`);
                }
                
                const result = await response.json();
                const summary = result.candidates?.[0]?.content?.parts?.[0]?.text;

                if (summary) {
                    // We'll use the message box to show the summary
                    const modalMessage = `<div class="p-4 bg-gray-800 rounded-lg shadow-lg">
                                            <h4 class="text-xl font-bold mb-2">Project Summary</h4>
                                            <p class="text-sm">${summary}</p>
                                        </div>`;
                    showMessage(summary, 'success');
                } else {
                    throw new Error("No summary returned from LLM.");
                }
            } catch (error) {
                console.error("Error summarizing project with LLM: ", error);
                showMessage("Failed to generate summary.", "error");
            }
        };

        // Authentication State Listener
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                userId = user.uid;
                userIdDisplay.textContent = userId;
                loadingMessage.style.display = 'none';
                attachFirestoreListener();
            } else {
                try {
                    if (initialAuthToken) {
                        await signInWithCustomToken(auth, initialAuthToken);
                    } else {
                        await signInAnonymously(auth);
                    }
                } catch (error) {
                    console.error("Firebase authentication failed:", error);
                    showMessage("Authentication failed. Check the console.", "error");
                    userIdDisplay.textContent = 'Auth Failed';
                    loadingMessage.style.display = 'none';
                    projectsContainer.innerHTML = `<div class="text-center text-red-500 mt-10">Authentication failed. Please check your network and try again.</div>`;
                }
            }
        });

        // Firestore Operations
        const getProjectsCollectionRef = () => {
            if (!userId) {
                console.error("User ID is not set. Cannot get Firestore collection.");
                showMessage("Authentication failed. Cannot save projects.", "error");
                return null;
            }
            return collection(db, `artifacts/${appId}/users/${userId}/projects`);
        };

        const addProject = async (projectName) => {
            const projectsRef = getProjectsCollectionRef();
            if (!projectsRef) return;
            try {
                await addDoc(projectsRef, {
                    name: projectName,
                    createdAt: new Date(),
                    tasks: []
                });
                newProjectNameInput.value = '';
                showMessage("Project added!", "success");
            } catch (error) {
                console.error("Error adding project: ", error);
                showMessage("Failed to add project.", "error");
            }
        };
        
        const deleteProject = async (projectId) => {
            const projectsRef = getProjectsCollectionRef();
            if (!projectsRef) return;
            try {
                const projectDocRef = doc(projectsRef, projectId);
                await deleteDoc(projectDocRef);
                showMessage("Project deleted successfully!", "success");
            } catch (error) {
                console.error("Error deleting project: ", error);
                showMessage("Failed to delete project.", "error");
            }
        };

        const addTask = async (projectId, taskDescription) => {
            const projectsRef = getProjectsCollectionRef();
            if (!projectsRef) return;
            try {
                const projectDocRef = doc(projectsRef, projectId);
                const docSnap = await getDoc(projectDocRef);
                
                if (docSnap.exists()) {
                    const currentTasks = docSnap.data().tasks || [];
                    const newTask = { description: taskDescription, completed: false, createdAt: new Date() };
                    await updateDoc(projectDocRef, {
                        tasks: [...currentTasks, newTask]
                    });
                    showMessage("Task added!", "success");
                } else {
                    console.error("Project document not found.");
                }
            } catch (error) {
                console.error("Error adding task: ", error);
                showMessage("Failed to add task.", "error");
            }
        };

        const toggleTaskCompleted = async (projectId, taskIndex, isCompleted) => {
            const projectsRef = getProjectsCollectionRef();
            if (!projectsRef) return;
            try {
                const projectDocRef = doc(projectsRef, projectId);
                const docSnap = await getDoc(projectDocRef);

                if (docSnap.exists()) {
                    const currentTasks = docSnap.data().tasks || [];
                    if (taskIndex >= 0 && taskIndex < currentTasks.length) {
                        currentTasks[taskIndex].completed = isCompleted;
                        currentTasks[taskIndex].completedAt = isCompleted ? new Date() : null;
                        await updateDoc(projectDocRef, {
                            tasks: currentTasks
                        });
                        showMessage("Task status updated!", "success");
                    }
                } else {
                    console.error("Project document not found.");
                }
            } catch (error) {
                console.error("Error updating task status: ", error);
                showMessage("Failed to update task.", "error");
            }
        };

        const deleteTask = async (projectId, taskIndex) => {
            const projectsRef = getProjectsCollectionRef();
            if (!projectsRef) return;
            try {
                const projectDocRef = doc(projectsRef, projectId);
                const docSnap = await getDoc(projectDocRef);

                if (docSnap.exists()) {
                    const currentTasks = docSnap.data().tasks || [];
                    if (taskIndex >= 0 && taskIndex < currentTasks.length) {
                        const newTasks = currentTasks.filter((_, index) => index !== taskIndex);
                        await updateDoc(projectDocRef, { tasks: newTasks });
                        showMessage("Task deleted!", "success");
                    }
                } else {
                    console.error("Project document not found.");
                }
            } catch (error) {
                console.error("Error deleting task: ", error);
                showMessage("Failed to delete task.", "error");
            }
        };

        const editTask = async (projectId, taskIndex, newDescription) => {
            const projectsRef = getProjectsCollectionRef();
            if (!projectsRef) return;
            try {
                const projectDocRef = doc(projectsRef, projectId);
                const docSnap = await getDoc(projectDocRef);

                if (docSnap.exists()) {
                    const currentTasks = docSnap.data().tasks || [];
                    if (taskIndex >= 0 && taskIndex < currentTasks.length) {
                        currentTasks[taskIndex].description = newDescription;
                        await updateDoc(projectDocRef, { tasks: currentTasks });
                        showMessage("Task updated!", "success");
                    }
                } else {
                    console.error("Project document not found.");
                }
            } catch (error) {
                console.error("Error editing task: ", error);
                showMessage("Failed to edit task.", "error");
            }
        };

        // UI Rendering
        const renderProjects = (projects) => {
            projectsContainer.innerHTML = '';
            const searchQuery = searchInput.value.toLowerCase();
            
            const filteredProjects = projects.filter(project => {
                const projectName = project.name.toLowerCase();
                const tasksMatch = project.tasks.some(task => task.description.toLowerCase().includes(searchQuery));
                return projectName.includes(searchQuery) || tasksMatch;
            });
            
            if (filteredProjects.length === 0) {
                projectsContainer.innerHTML = '<div class="text-center text-gray-400 mt-10">No projects found.</div>';
                return;
            }

            filteredProjects.forEach(project => {
                const totalTasks = project.tasks.length;
                const completedTasks = project.tasks.filter(task => task.completed).length;
                const progress = totalTasks > 0 ? Math.round((completedTasks / totalTasks) * 100) : 0;
                const projectCreationDate = project.createdAt ? new Date(project.createdAt.seconds * 1000).toLocaleDateString() : 'N/A';

                const projectCard = document.createElement('div');
                projectCard.className = 'card p-6 mb-6';
                projectCard.innerHTML = `
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-xl font-semibold text-gray-50">${project.name}</h3>
                        <div class="flex space-x-2 items-center">
                            <button class="btn bg-gray-700 hover:bg-gray-600 text-white text-xs px-2 py-1 rounded-full generate-tasks-btn" data-project-id="${project.id}" data-project-name="${project.name}">Generate Tasks ✨</button>
                            <button class="btn bg-gray-700 hover:bg-gray-600 text-white text-xs px-2 py-1 rounded-full summarize-project-btn" data-project-id="${project.id}" data-project-name="${project.name}">Summarize Project ✨</button>
                            <button class="icon-btn delete-project-btn" data-project-id="${project.id}">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.035 21H7.965a2 2 0 01-1.998-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                                </svg>
                            </button>
                        </div>
                    </div>
                    <div class="mb-4">
                        <p class="text-sm text-gray-400">Created: ${projectCreationDate}</p>
                        <div class="w-full bg-gray-700 rounded-full h-2.5 mt-2">
                            <div class="bg-blue-600 h-2.5 rounded-full" style="width: ${progress}%"></div>
                        </div>
                        <p class="text-right text-xs text-gray-400 mt-1">${progress}% Completed</p>
                    </div>
                    <ul class="task-list space-y-2 mb-4">
                        ${project.tasks.map((task, index) => {
                            const taskCreationDate = task.createdAt ? new Date(task.createdAt.seconds * 1000).toLocaleDateString() : 'N/A';
                            const taskCompletionDate = task.completedAt ? new Date(task.completedAt.seconds * 1000).toLocaleDateString() : '';
                            return `
                                <li class="task-item group">
                                    <input type="checkbox" data-project-id="${project.id}" data-task-index="${index}" class="task-checkbox" ${task.completed ? 'checked' : ''}>
                                    <span class="task-text ${task.completed ? 'line-through text-gray-500' : 'text-gray-300'}">${task.description}</span>
                                    <span class="flex-grow"></span>
                                    ${task.completed ? `<span class="text-xs text-gray-500 mr-2">Completed: ${taskCompletionDate}</span>` : ''}
                                    <button class="icon-btn edit-task-btn invisible group-hover:visible mr-2" data-project-id="${project.id}" data-task-index="${index}">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-yellow-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
                                        </svg>
                                    </button>
                                    <button class="icon-btn delete-task-btn invisible group-hover:visible" data-project-id="${project.id}" data-task-index="${index}">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                                        </svg>
                                    </button>
                                </li>
                            `;
                        }).join('')}
                    </ul>
                    <div class="flex space-x-2">
                        <input type="text" class="input-field addTaskInput w-full p-2 rounded-lg" placeholder="Add a new task...">
                        <button class="btn bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg" data-project-id="${project.id}">Add Task</button>
                    </div>
                `;
                projectsContainer.appendChild(projectCard);
            });

            // Add event listeners for new buttons and checkboxes
            projectsContainer.querySelectorAll('.task-checkbox').forEach(checkbox => {
                checkbox.addEventListener('change', (event) => {
                    const projectId = event.target.dataset.projectId;
                    const taskIndex = parseInt(event.target.dataset.taskIndex);
                    toggleTaskCompleted(projectId, taskIndex, event.target.checked);
                });
            });

            projectsContainer.querySelectorAll('.generate-tasks-btn').forEach(button => {
                button.addEventListener('click', (event) => {
                    const projectId = event.target.dataset.projectId;
                    const projectName = event.target.dataset.projectName;
                    generateTasksWithLLM(projectName, projectId);
                });
            });

            projectsContainer.querySelectorAll('.summarize-project-btn').forEach(button => {
                button.addEventListener('click', (event) => {
                    const projectId = event.target.dataset.projectId;
                    const project = allProjects.find(p => p.id === projectId);
                    if (project) {
                        summarizeProjectWithLLM(project);
                    }
                });
            });

            projectsContainer.querySelectorAll('button[data-project-id]').forEach(button => {
                if (button.classList.contains('delete-project-btn')) {
                    button.addEventListener('click', (event) => {
                        const projectId = event.target.closest('button').dataset.projectId;
                        deleteProject(projectId);
                    });
                } else if (button.classList.contains('delete-task-btn')) {
                    button.addEventListener('click', (event) => {
                        const projectId = event.target.closest('button').dataset.projectId;
                        const taskIndex = parseInt(event.target.closest('button').dataset.taskIndex);
                        deleteTask(projectId, taskIndex);
                    });
                } else if (button.classList.contains('edit-task-btn')) {
                    button.addEventListener('click', (event) => {
                        const button = event.target.closest('button');
                        const projectId = button.dataset.projectId;
                        const taskIndex = parseInt(button.dataset.taskIndex);
                        const taskItem = button.closest('li');
                        const taskTextSpan = taskItem.querySelector('.task-text');
                        const currentDescription = taskTextSpan.textContent;

                        const input = document.createElement('input');
                        input.type = 'text';
                        input.value = currentDescription;
                        input.className = 'input-field w-full p-1 rounded-lg mr-2';
                        taskTextSpan.replaceWith(input);

                        const saveBtn = document.createElement('button');
                        saveBtn.textContent = 'Save';
                        saveBtn.className = 'btn bg-green-600 text-white p-2 rounded-lg';
                        saveBtn.onclick = () => {
                            const newDescription = input.value.trim();
                            if (newDescription) {
                                editTask(projectId, taskIndex, newDescription);
                            }
                        };
                        button.replaceWith(saveBtn);

                        input.focus();
                    });
                } else {
                    button.addEventListener('click', (event) => {
                        const clickedButton = event.target.closest('button');
                        if (!clickedButton) return;
                        const projectId = clickedButton.dataset.projectId;
                        const input = clickedButton.previousElementSibling;
                        const taskDescription = input.value.trim();
                        if (taskDescription) {
                            addTask(projectId, taskDescription);
                            input.value = '';
                        }
                    });
                }
            });
        };

        // Main Firestore Listener
        const attachFirestoreListener = () => {
            const projectsRef = getProjectsCollectionRef();
            if (!projectsRef) {
                console.log("Projects ref is null. Skipping listener attachment.");
                return;
            }

            onSnapshot(query(projectsRef), (snapshot) => {
                allProjects = [];
                snapshot.forEach(doc => {
                    allProjects.push({ id: doc.id, ...doc.data() });
                });
                renderProjects(allProjects);
            }, (error) => {
                console.error("Error getting projects: ", error);
                showMessage("Failed to load projects. Check the console for more details.", "error");
            });
        };

        // Event Listeners
        addProjectBtn.addEventListener('click', () => {
            const projectName = newProjectNameInput.value.trim();
            if (projectName) {
                addProject(projectName);
            }
        });

        searchInput.addEventListener('input', () => {
            renderProjects(allProjects);
        });

        // Initial Auth State Check
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                userId = user.uid;
                userIdDisplay.textContent = userId;
                loadingMessage.style.display = 'none';
                attachFirestoreListener();
            } else {
                try {
                    if (initialAuthToken) {
                        await signInWithCustomToken(auth, initialAuthToken);
                    } else {
                        await signInAnonymously(auth);
                    }
                } catch (error) {
                    console.error("Firebase authentication failed:", error);
                    showMessage("Authentication failed. Check the console.", "error");
                    userIdDisplay.textContent = 'Auth Failed';
                    loadingMessage.style.display = 'none';
                    projectsContainer.innerHTML = `<div class="text-center text-red-500 mt-10">Authentication failed. Please check your network and try again.</div>`;
                }
            }
        });
    </script>
</body>
</html>
