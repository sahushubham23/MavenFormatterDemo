@ExtendWith(MockitoExtension.class)
class Cremb003ServiceImplTest {

    @InjectMocks
    private Cremb003ServiceImpl service;

    @Mock
    private Gartsurgr1Repository gartsurgr1Repository;
    @Mock
    private Gartsurgr2Repository gartsurgr2Repository;
    @Mock
    private Gartsurgr3Repository gartsurgr3Repository;
    @Mock
    private Gartsurgr4Repository gartsurgr4Repository;

    @Spy
    private Cremb003ServiceImpl spyService;

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
        doReturn(400).when(spyService).stdsurctypobj(101);
        when(gartsurgr1Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void case401() {
        doReturn(401).when(spyService).stdsurctypobj(101);
        when(gartsurgr2Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void case402() {
        doReturn(402).when(spyService).stdsurctypobj(101);
        when(gartsurgr3Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void case403() {
        doReturn(403).when(spyService).stdsurctypobj(101);
        when(gartsurgr4Repository.getExistCouche1(101, 10, 30L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void defaultIfTrue() {
        doReturn(null).when(spyService).stdsurctypobj(101);
        when(gartsurgr1Repository.getExistCouche(101, 10, 20L)).thenReturn(1);

        assertTrue(spyService.openWindowAction(param));
    }

    @Test
    void defaultIfFalse() {
        doReturn(null).when(spyService).stdsurctypobj(101);
        param.setCtypdos(Constants.YES);

        assertFalse(spyService.openWindowAction(param));
    }
}
------------------------------

@ExtendWith(MockitoExtension.class)
class Cremb003ServiceImplTest {

    @InjectMocks
    private Cremb003ServiceImpl service;

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
        tenvlog.setCsitenv(1);
        tenvlog.setNmtrenv(2);
        tenvlog.setNmtrvalid(1);
    }

    /**
     * ❌ tenvlog null → exception
     */
    @Test
    void hideDialogWindowAction_whenNoEnvlog_shouldThrowException() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(null);

        assertThrows(
                CREClientException.class,
                () -> service.hideDialogWindowAction(1, "NSUR", 101)
        );
    }

    /**
     * ✅ nsur = null (no wcount lookup)
     */
    @Test
    void hideDialogWindowAction_whenNsurNull() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);

        BlocksCommonResponse response =
                service.hideDialogWindowAction(1, null, 101);

        assertNotNull(response);
        assertEquals(1, response.getTenvlogDtos().size());
        assertNull(response.getTenvlogDtos().get(0).getWcount());

        verifyNoInteractions(
                gartsurgr1Repository,
                gartsurgr2Repository,
                gartsurgr3Repository
        );
    }

    /**
     * ✅ ctypsur group-1
     */
    @Test
    void hideDialogWindowAction_group1() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);
        when(gartsurgr1Repository.getWcount("NSUR"))
                .thenReturn(5);

        BlocksCommonResponse response =
                service.hideDialogWindowAction(1, "NSUR", 101);

        assertEquals(
                5,
                response.getTenvlogDtos().get(0).getWcount()
        );
    }

    /**
     * ✅ ctypsur group-2
     */
    @Test
    void hideDialogWindowAction_group2() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);
        when(gartsurgr2Repository.getWcount("NSUR"))
                .thenReturn(7);

        BlocksCommonResponse response =
                service.hideDialogWindowAction(1, "NSUR", 202);

        assertEquals(
                7,
                response.getTenvlogDtos().get(0).getWcount()
        );
    }

    /**
     * ✅ ctypsur group-3
     */
    @Test
    void hideDialogWindowAction_group3() {

        when(gartEnvlogRepository.usrSurCall(1))
                .thenReturn(tenvlog);
        when(gartsurgr3Repository.getWcount("NSUR"))
                .thenReturn(9);

        BlocksCommonResponse response =
                service.hideDialogWindowAction(1, "NSUR", 311);

        assertEquals(
                9,
                response.getTenvlogDtos().get(0).getWcount()
        );
    }
}
