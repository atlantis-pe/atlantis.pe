# API REST EXAMPLE
## Como crear tu servicio en Angular

### Ejemplo:

```typescript
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Injectable } from '@angular/core';
import { Filter } from '../entities/filter';
import { Sorter } from '../entities/sorter';

@Injectable({
  providedIn: 'root',
})
export class HttpGetListService {
  private pageParam = 'page';
  private limitParam = 'limit';
  private sorterParam = 'sorter';
  private filterParam = 'filter';
  constructor(private http: HttpClient) {}
  /**
   * Realiza una petición GET
   */
  get<T>(
    url: string,
    page?: number,
    limit?: number,
    sorters?: Sorter[],
    filters?: Filter[]
  ): Observable<T> {
    const me = this;
    const queryParams = [];
    if (page) {
      queryParams.push(`${me.pageParam}=${page}`);
    }
    if (limit) {
      queryParams.push(`${me.limitParam}=${limit}`);
    }
    if (sorters) {
      queryParams.push(`${me.sorterParam}=${me.encodeSorters(sorters)}`);
    }
    if (filters) {
      queryParams.push(`${me.filterParam}=${me.encodeFilters(filters)}`);
    }
    url = me.urlAppend(url, queryParams.join('&'));
    return me.http.get<T>(url);
  }
  urlAppend(url, str) {
    if (str) {
      return url + (url.indexOf('?') == -1 ? '?' : '&') + str;
    }
    return url;
  }
  encodeSorters(sorters: Sorter[]) {
    return JSON.stringify(sorters);
  }
  encodeFilters(filters: Filter[]) {
    return JSON.stringify(filters);
  }
}

```