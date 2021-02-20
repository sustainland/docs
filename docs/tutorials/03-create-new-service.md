---
id: create-new-service
title: 03-Create Service
---

Now we have our controller let's add a service to manipulate data : 
```typescript
import { Project } from './../models/project.model';
import { Injectable } from "@sustain/core";

@Injectable()
export class ProjectService {
    projects: Project[] = [];

    add(project: Project): void {
        this.projects.push(project);
    }

    removeById(projectId: string) {
        return this.projects = this.projects.filter((project: any) => project.id != projectId);
    }

    list(): Project[] {
        return this.projects;
    }
}
```

We also create the model and we call it ``Project``
```typescript
export class Project {
    id: string;
    name: string;
    description: string;
    start?: Date;
    due?: Date;
}

```

Next step is to use our service in the controller

```typescript
import { ProjectService } from './../services/ProjectService';
import { Project } from './../models/project.model';
import { Get, Post, Body, Delete, Param } from "@sustain/common";
import { Controller } from "@sustain/core";


@Controller('/project')
export default class ProjectController {
    constructor(
        private projectService: ProjectService // <-- Injected in the contructor
    ) { }

    @Get()
    listProject() {
        return this.projectService.list(); // use it here
    }

    @Post()
    createProject(@Body() project: Project) {
        this.projectService.add(project); // here
        return 'ADDED';
    }

    @Delete('/:id')
    deleteProject(@Param('id') projectId: string) {
        this.projectService.removeById(projectId); // and here
        return 'DELETED';
    }
}
```

Did we forget something ? **Yes**. 

We need to important the service into ``App.ts`` to let now the Injector that we have new service.

```typescript
...
@App({
    controllers: [
        HelloController,
        ProjectController
    ],
    providers: [
        ProjectService // <- added here
    ]
})
...

```

And the Final folder structure
```ts
.
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo-text.png
│   └── logo.png
├── src
│   ├── app.ts
│   ├── controllers
│   │   └── ProjectController.ts
│   ├── models
│   │   └── project.model.ts
│   └── services
│       └── ProjectService.ts
└── tsconfig.json
```

You can find the source code of this tutorial in this repository : https://github.com/sustainland/tutorials