## Guidance
Answer the following questions considering the learning outcomes for
- [Week 011](https://learn.foundersandcoders.com/course/syllabus/developer/week11-project05-DOTNET-testing/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * My focus this week has been less on interfaces and dependency injections as I set up most of my code base in C# last week, however I have a grasp of it and used the predefiend ICollections interface to pull up pokemon from another model into the collection model (although there is no dependency injection):
```csharp
public ICollection<PokemonCollections> PokemonCollections { get; set; } = new List<PokemonCollections>();
```
 * I also was not able to explore more about testing in C# using xUnit using mock data and without changing the database - this I hope to get into on the next project.
 * Sucessfully connected our front end with the API using my standard fetchData function in my [React template](https://github.com/viatora/viatora-template-react), even connected in local to our deployed backend:
```typescript
export async function fetchData(
  query: string,
  method: string = "GET",
  body?: string
): Promise<any> {
  try {
    const response = await fetch(`http://34.105.229.117:8080/${query}`, {
      method,
      headers: {
        "Content-Type": "application/json",
      },
      ...(body && { body }),
    });

    if (!response.ok) {
      console.error("HTTP error:", response.status, response.statusText);
      throw new Error(`HTTP error! Status: ${response.status}`);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error fetching data:", error);
    throw error;
  }
}
```
 * Learning to use `Dockerfiles` and `docker-compose.yml` to set up containers locally using Docker Desktop, the `Dockerfile` sets up instructions to run when setting up the container such as:
```Dockerfiles
COPY *.csproj ./
RUN dotnet restore
```
While the `docker-compose.yml` file sets up the container configurations such as for the database:
```yml
db:
    image: postgres:latest
    container_name: pokelike_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: welovepokemon
      POSTGRES_DB: PokeLikeDb
    ports:
      - "5433:5432"
    restart: always
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
    networks:
      - pokelike-network
```
 * Learned to use Kubernetes Engine on Google Cloud for deployment, using Docker network and Kubernetes cluster, this meant setting up more yaml files called Kubernetes Manifests whichs set up the deployment environment for the containers, example for api deployment:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pokelike-api
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pokelike-api
  template:
    metadata:
      labels:
        app: pokelike-api
    spec:
      containers:
        - name: pokelike-api
          image: europe-west2-docker.pkg.dev/poke-like-api/pokelike-artifacts/pokelike-artifacts:v1
          ports:
            - containerPort: 8080
          env:
            - name: ASPNETCORE_URLS
              value: "http://+:8080"
            - name: ConnectionStrings__PokeLikeDbContext
              value: "Host=postgres;Port=5432;Database=PokeLikeDb;Username=postgres;Password=welovepokemon"
```

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * I did struggle with the GCP deployment on Kubernetes Engine as well as with Docker in the first place, one such example was that I thought the containers would set up migrations aswell which it did not, and so I was supposed to run `dotnet ef migrations add InitialCreate` before starting the `docker build` commands which would then handle the rest of the dotnet commands for you within the container itself.
 * One thing I struggled with too are ports, example for the database, I believe because the containers itself runs on the required port, you need to set another port within the yaml file and remap it to the standard postgres port of `5432`, so the yaml file had to be set up like this:
```yaml
ports:
      - "5433:5432"
```
## Feedback (For CF's)
> [**Course Facilitator name**]  
> [*What went well*]  
> [*Even better if*]
