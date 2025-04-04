Ways to create mock:
 1. using mock() method:
      List mockList = Mockito.mock(ArrayList.class);
 2. using @Mock annotation: 
       @Mock
       List<String> mockList;
   3. In Spring framework we can also use @MockBean annotation to create a mock of any object and inject it in the application context:
         @MockBean
         EmployeeRepository emloyeeRepoMock;
 