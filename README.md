@ExtendWith(MockitoExtension.class)
class Crembo03ServiceImplTest {

    @InjectMocks
    private Crembo03ServiceImpl service;

    @Mock
    private Gartsurgr1Repository gartsurgr1Repository;

    @Mock
    private Gartsurgr2Repository gartsurgr2Repository;

    @Mock
    private Gartsurgr3Repository gartsurgr3Repository;

    @Mock
    private Gartsurgr4Repository gartsurgr4Repository;

    @Spy
    private Crembo03ServiceImpl spyService;

    private Crembo03ExistCountParam param;

    @BeforeEach
    void setup() {
        param = new Crembo03ExistCountParam();
        param.setCtypsur(101);
        param.setNouvsur(10);
        param.setNitv(20L);
        param.setNdos(30L);
        param.setCtypdos(Constants.NO);
        param.setCtypeve(2);
    }

    /**
     * case 400 → gartsurgr1Repository.getExistCouche1
     */
    @Test
    void openWindowAction_case400() {

        doReturn(400).when(spyService).stdsurctypobj(101);
        when(gartsurgr1Repository.getExistCouche1(101, 10, 30L))
                .thenReturn(1);

        Boolean result = spyService.openWindowAction(param);

        assertTrue(result);
    }

    /**
     * case 401 → gartsurgr2Repository
     */
    @Test
    void openWindowAction_case401() {

        doReturn(401).when(spyService).stdsurctypobj(101);
        when(gartsurgr2Repository.getExistCouche1(101, 10, 30L))
                .thenReturn(1);

        Boolean result = spyService.openWindowAction(param);

        assertTrue(result);
    }

    /**
     * case 402 → gartsurgr3Repository
     */
    @Test
    void openWindowAction_case402() {

        doReturn(402).when(spyService).stdsurctypobj(101);
        when(gartsurgr3Repository.getExistCouche1(101, 10, 30L))
                .thenReturn(1);

        Boolean result = spyService.openWindowAction(param);

        assertTrue(result);
    }

    /**
     * case 403 → gartsurgr4Repository
     */
    @Test
    void openWindowAction_case403() {

        doReturn(403).when(spyService).stdsurctypobj(101);
        when(gartsurgr4Repository.getExistCouche1(101, 10, 30L))
                .thenReturn(1);

        Boolean result = spyService.openWindowAction(param);

        assertTrue(result);
    }

    /**
     * default → inner IF true
     */
    @Test
    void openWindowAction_default_innerIfTrue() {

        doReturn(null).when(spyService).stdsurctypobj(101);
        when(gartsurgr1Repository.getExistCouche(101, 10, 20L))
                .thenReturn(1);

        Boolean result = spyService.openWindowAction(param);

        assertTrue(result);
    }

    /**
     * default → inner IF false
     */
    @Test
    void openWindowAction_default_innerIfFalse() {

        doReturn(null).when(spyService).stdsurctypobj(101);
        param.setCtypdos(Constants.YES); // breaks IF condition

        Boolean result = spyService.openWindowAction(param);

        assertFalse(result);
    }

}
-------------------------------

@ExtendWith(MockitoExtension.class)
class BselsurServiceImplTest {

    @InjectMocks
    private BselsurServiceImpl service;

    @Mock
    private GartEnvlogRepository gartEnvlogRepository;

    @Mock
    private Gartsurgr1Repository gartsurgr1Repository;

    @Mock
    private Gartsurgr2Repository gartsurgr2Repository;

    @Mock
    private Gartsurgr3Repository gartsurgr3Repository;

    @Mock
    private Dspmsg dspmsg;

    private TenvLog tenvlog;

    @BeforeEach
    void setup() {
        tenvlog = new TenvLog();
        tenvlog.setCsitenv("SITE");
        tenvlog.setMtrenv("MTR");
        tenvlog.setEnvlogid(10L);
        tenvlog.setMtrvalid("Y");
    }

    /**
     * ❌ tenvlog is null → exception
     */
    @Test
    void hideDialogWindowAction_whenEnvlogNull_shouldThrowException() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(null);

        assertThrows(
                CREClientException.class,
                () -> service.hideDialogWindowAction(1, "NSUR", 101)
        );
    }

    /**
     * ✅ nsur = null (no repo calls for wcount)
     */
    @Test
    void hideDialogWindowAction_whenNsurNull() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);

        BlockCommonResponse response =
                service.hideDialogWindowAction(1, null, 101);

        assertNotNull(response);
        assertEquals(1, response.getTenvlogdto().size());

        verifyNoInteractions(gartsurgr1Repository,
                             gartsurgr2Repository,
                             gartsurgr3Repository);
    }

    /**
     * ✅ ctypsur group-1 → gartsurgr1Repository
     */
    @Test
    void hideDialogWindowAction_ctypsurGroup1() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);
        when(gartsurgr1Repository.getWcount("NSUR"))
                .thenReturn(5);

        BlockCommonResponse response =
                service.hideDialogWindowAction(1, "NSUR", 101);

        assertEquals(5,
                response.getTenvlogdto().get(0).getWcount());
    }

    /**
     * ✅ ctypsur group-2 → gartsurgr2Repository
     */
    @Test
    void hideDialogWindowAction_ctypsurGroup2() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);
        when(gartsurgr2Repository.getWcount("NSUR"))
                .thenReturn(7);

        BlockCommonResponse response =
                service.hideDialogWindowAction(1, "NSUR", 202);

        assertEquals(7,
                response.getTenvlogdto().get(0).getWcount());
    }

    /**
     * ✅ ctypsur group-3 → gartsurgr3Repository
     */
    @Test
    void hideDialogWindowAction_ctypsurGroup3() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);
        when(gartsurgr3Repository.getWcount("NSUR"))
                .thenReturn(9);

        BlockCommonResponse response =
                service.hideDialogWindowAction(1, "NSUR", 311);

        assertEquals(9,
                response.getTenvlogdto().get(0).getWcount());
    }
}


---------------------------------

@ExtendWith(MockitoExtension.class)
class BselsurServiceImplTest {

    @InjectMocks
    private BselsurServiceImpl service;

    @Mock
    private GartObjphyRepository gartObjphyRepository;

    @Mock
    private MessageService messageService;

    @Mock
    private Dspmsg dspmsg;

    /**
     * ✅ Success case
     * Repository returns value
     */
    @Test
    void hideWindowAction_success() {

        Long wndos = 100L;

        when(gartObjphyRepository.usrSurCall(wndos))
                .thenReturn(1);

        Integer result = service.hideWindowAction(wndos);

        assertNotNull(result);
        assertEquals(1, result);

        verify(gartObjphyRepository, times(1))
                .usrSurCall(wndos);
    }

    /**
     * ❌ Repository returns null
     * ❌ CREClientException thrown
     */
    @Test
    void hideWindowAction_whenNoDataFound_shouldThrowException() {

        Long wndos = 200L;

        when(gartObjphyRepository.usrSurCall(wndos))
                .thenReturn(null);

        assertThrows(
                CREClientException.class,
                () -> service.hideWindowAction(wndos)
        );

        verify(gartObjphyRepository, times(1))
                .usrSurCall(wndos);
    }
}
