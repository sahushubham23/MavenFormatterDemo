import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.Optional;

@Service
public class GdsDbMenuService {

    private final GdsDbMenuRepository gdsDbMenuRepository;

    public GdsDbMenuService(GdsDbMenuRepository gdsDbMenuRepository) {
        this.gdsDbMenuRepository = gdsDbMenuRepository;
    }

    @Transactional
    public GdsDbMenu saveOrUpdateMenu(GdsDbMenu menu) {
        return gdsDbMenuRepository.save(menu);
    }

    public Optional<GdsDbMenu> getMenuByRootId(Long rootId) {
        return gdsDbMenuRepository.findById(rootId);
    }

    @Transactional
    public boolean deleteByRootId(Long rootId) {
        if (gdsDbMenuRepository.existsById(rootId)) {
            gdsDbMenuRepository.deleteById(rootId);
            return true;
        }
        return false;
    }
}

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.Optional;

@RestController
@RequestMapping("/api/gdsdbmenu")
public class GdsDbMenuController {

    private final GdsDbMenuService gdsDbMenuService;

    public GdsDbMenuController(GdsDbMenuService gdsDbMenuService) {
        this.gdsDbMenuService = gdsDbMenuService;
    }

    // Insert or update a record
    @PostMapping("/save")
    public ResponseEntity<GdsDbMenu> saveOrUpdate(@RequestBody GdsDbMenu menu) {
        GdsDbMenu savedMenu = gdsDbMenuService.saveOrUpdateMenu(menu);
        return ResponseEntity.ok(savedMenu);
    }

    // Fetch a record by rootId
    @GetMapping("/{rootId}")
    public ResponseEntity<GdsDbMenu> getByRootId(@PathVariable Long rootId) {
        Optional<GdsDbMenu> menu = gdsDbMenuService.getMenuByRootId(rootId);
        return menu.map(ResponseEntity::ok)
                   .orElseGet(() -> ResponseEntity.notFound().build());
    }

    // Delete a record by rootId
    @DeleteMapping("/{rootId}")
    public ResponseEntity<String> deleteByRootId(@PathVariable Long rootId) {
        boolean isDeleted = gdsDbMenuService.deleteByRootId(rootId);
        if (isDeleted) {
            return ResponseEntity.ok("Record with rootId " + rootId + " deleted successfully.");
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}

DELETE /api/gdsdbmenu/1

