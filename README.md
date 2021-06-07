# Operation Name Mock Link

## Differences with default Apollo MockedProvider

`MockedProvider` has a very strict matching behaviour: your query variables and the mock variables
must match EXACTLY, including keys with the value `undefined`.
It makes tests very bristle and leads to the dreaded `No more mocked responses for the query:` error.

This package uses a lighter approach, mocks are matched only based on the `operationName`.

Limitation: this might be problematic if a component is triggering multiple queries with the same `operationName`.
But it is fine in most scenarios.

## Usage

```tsx
import { MockedProvider } from "@apollo/client/testing";
import {
  OperationNameMockedResponse,
  OperationNameMockLink,
  
} from "operation-name-mock-link";

interface GetSomeDataResult {
  getSomeData: {
    id: string;
  }
}

const mock: OperationNameMockedResponse<GetSomeDataResult> = {
  request: {
    operationName: "myQuery",
    query: gql`
      query myQuery {
        getSomeData {
          id
        }
      }`
  },
  result: {
    data: {
      getSomeData: {
          id: "42"
      }
    }
  }
};

const wrapper = ({ children }) => (
  <MockedProvider
    // We replace MockedProvider default link with our custom MockLink
    link={new OperationNameMockLink([mock], false)}
  >
    {children}
  </MockedProvider>
);
```
