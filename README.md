@ExtendWith(MockitoExtension.class)
class Cremb003ServiceImplTest {

    @InjectMocks
    private Cremb003ServiceImpl service;

    @Spy
    private Cremb003ServiceImpl spyService;

    @Mock
    private Gartsurgr1Repository gartsurgr1Repository;
    @Mock
    private Gartsurgr2Repository gartsurgr2Repository;
    @Mock
    private Gartsurgr3Repository gartsurgr3Repository;
    @Mock
    private Gartsurgr4Repository gartsurgr4Repository;

    private Cremb003ExistCouche param;

    @BeforeEach
    void setup() {
        param = new Cremb003ExistCouche();
        param.setCtypsur(101);
        param.setNouvsur(10);
        param.setNitv(20L);
        param.setNdos(30L);
        param.setCtypdos(Constants.NO);
        param.setCtypeve(2);
    }

    @Test
    void case400() {
        doReturn(400).when(spyService).stdSurCtypobj(101);
        when(gartsurgr1Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void case401() {
        doReturn(401).when(spyService).stdSurCtypobj(101);
        when(gartsurgr2Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void case402() {
        doReturn(402).when(spyService).stdSurCtypobj(101);
        when(gartsurgr3Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void case403() {
        doReturn(403).when(spyService).stdSurCtypobj(101);
        when(gartsurgr4Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void default_ifTrue() {
        doReturn(null).when(spyService).stdSurCtypobj(101);
        when(gartsurgr1Repository.getExistCouche(101, 10, 20L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void default_ifFalse() {
        doReturn(null).when(spyService).stdSurCtypobj(101);
        param.setCtypdos(Constants.YES);

        assertFalse(spyService.openWindowAction(param));
    }
}
