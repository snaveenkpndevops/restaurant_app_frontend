## Frontend (Angular) 

1. npm install -g @angular/cli@16.0.0  -->  Run the following command to install Angular CLI globally

2. ng new frontend --version=16.0.0    -->  Create Angular App named frontend

3. paste all the code (refer geeks for geeks documents)

   Reference from geeks for geeks : https://www.geeksforgeeks.org/restaurant-recommendation-app-using-mean/

4. ng serve  --> Start the Angular App using the following command.

5. our frontend is running in http://localhost:4200

6. In frontend inside app.component.ts we configured the backend ip and port inside getRestaurants().
      Backend is running in port 4000.

       getRestaurants() {
        this.http.get('http://localhost:4000/api/restaurants')


7. For kubernetes environment, we need to change the backend url for connection.

   * In `restaurant.service.ts` update the backend url based on kubernetes backend service name. For example, if your backend service is called `express-backend-service`, you can update the API URL in restaurant.service.ts as follows:

      // Change the apiUrl to the internal DNS if the frontend is inside the same Kubernetes cluster
     private apiUrl = 'http://express-backend-service.default.svc.cluster.local/api/restaurants';  // Internal Kubernetes DNS


   * Best practice is to create environment and get the private apiUrl from enviroment variables. so in `app.component.ts` and in `restaurant.service.ts`. 

         prerequiste:

            1. Create environment folder and environment.ts (for dev env) and environment.prod.ts (for prod env)

   * In `restaurant.service.ts` we need to import environment and update the private apiUrl.

      ```
      import { environment } from '../environments/environment';

      @Injectable({
         providedIn: 'root',
      })
      export class RestaurantService {
         //private apiUrl = 'http://localhost:3000/api/restaurants';
         // Change the apiUrl to the internal DNS if the frontend is inside the same Kubernetes cluster
         private apiUrl =     environment.apiUrl;  // Internal Kubernetes DNS

      ```

   * same like that in `app.component.ts` we need to import environment and update the private apiUrl.

      ```
      import { environment } from './environments/environment';

      getRestaurants() {
        this.http.get('environment.apiUrl').subscribe(

      ```

   * Note:

      1. We are using angular default port 4200 to access our frontend. To change the port we need to edit `angular.json` file.

8. if we want to customize our nginx port -->  create nginx.conf file and modify the port to 4200 and update the dockerfile.