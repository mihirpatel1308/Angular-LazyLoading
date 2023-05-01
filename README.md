<!-- For Employee Module -->
ng g m employee --routing

ng g c employee/employee-list

<!-- For User Module -->
ng g m user --routing

ng g c user/user-list

<!-- Eager Loading App routing module  -->
const routes: Routes = [
  {
    path: '',
    redirectTo: '',
    pathMatch: 'full'
  },
  {
    path: 'employee',
    component: EmployeeListComponent
  },
  {
    path: 'user',
    component: UserListComponent
  },
];

<!-- App Component -->
<nav [ngClass]="'parent-menu'">
  <ul>
    <li><a routerLink="/employee" routerLinkActive="active">Employee</a></li>
    <li><a routerLink="/user" routerLinkActive="active">User</a></li>
  </ul>
</nav>
<div [ngClass]="'parent-container'">
  <router-outlet></router-outlet>
</div>

<!--Lazy loaded routing module  -->
const routes: Routes = [
  {
    path: '',
    redirectTo: '',
    pathMatch: 'full'
  },
  {
    path: 'employee',
    loadChildren: () => import('./employee/employee.module').then(mod => mod.EmployeeModule)
  },
  {
    path: 'user',
    loadChildren: () => import('./user/user.module').then(mod => mod.UserModule)
  },
];

<!-- Preload all modules -->
RouterModule.forRoot(routes, {
    preloadingStrategy: PreloadAllModules
  })

  <!-- Custom pre load strategy  -->
  ng g s custom-preloading-strategy

  <!-- Update path -->
  {
    path: 'user', data: {preload: true},
    loadChildren: () => import('./user/user.module').then(mod => mod.UserModule)
  }

<!-- custom service ;  -->
export class CustomPreloadingStrategyService implements PreloadingStrategy {
  preload(route: Route, fn: () => Observable<any>): Observable<any> {
      if (route.data && route.data['preload']) {
        return fn();
      }
      return of(null);
    }
  }

  <!-- ROuter module update :  -->
  RouterModule.forRoot(routes, {
    preloadingStrategy: CustomPreloadingStrategyService
  })