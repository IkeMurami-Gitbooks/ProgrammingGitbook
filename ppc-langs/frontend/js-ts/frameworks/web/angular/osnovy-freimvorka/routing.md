# Routing

## Component Routing

```typescript
@NgModule({
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    RouterModule.forRoot([
      { path: '', component: ProductListComponent },
      { path: 'products/:productId', component: ProductDetailsComponent },
      { path: 'cart', component: CartComponent },
    ])
  ],
```

Где-то в html:

```markup
<a routerLink="/cart" class="button fancy-button">
  <i class="material-icons">shopping_cart</i>Checkout
</a>
```

## Module Routing

routing module

```typescript
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";


const routes: Routes = [
  { path: 'import', loadChildren: () => import('../import/import.module').then(m => m.ImportModule)},  // It is default route too: / -> /import
  { path: 'workspace', loadChildren: () => import('../workspace/workspace.module').then(m => m.WorkspaceModule)}
]

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class RootRoutingModule { }
```

root module

```typescript
@NgModule({
  declarations: [
    RootComponent
  ],
  imports: [
    BrowserModule,
    
    ImportModule,
    WorkspaceModule,
    RootRoutingModule
  ],
  providers: [],
  bootstrap: [RootComponent]
})
export class RootModule { }
```
