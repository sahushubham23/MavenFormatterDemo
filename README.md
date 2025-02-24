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

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import java.time.LocalDateTime;

@Entity
@Getter
@Setter
@Table(name = "gdsmenuchoice")
public class GdsMenuChoice {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Auto-generated if applicable
    @Column(name = "id", nullable = false, updatable = false)
    private Long id;

    @Column(name = "rootid", nullable = false)
    private Long rootId;

    @Column(name = "choice_name")
    private String choiceName;

    @Column(name = "choice_description")
    private String choiceDescription;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    // Add remaining columns as per your database schema
}


import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

public interface GdsMenuChoiceRepository extends JpaRepository<GdsMenuChoice, Long> {

    List<GdsMenuChoice> findByRootId(Long rootId);

    @Transactional
    void deleteByRootIdAndId(Long rootId, Long id);

    @Transactional
    void deleteByRootId(Long rootId);
}


import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
public class GdsMenuChoiceService {

    private final GdsMenuChoiceRepository repository;

    public GdsMenuChoiceService(GdsMenuChoiceRepository repository) {
        this.repository = repository;
    }

    // Bulk insert or update
    @Transactional
    public List<GdsMenuChoice> saveOrUpdateChoices(List<GdsMenuChoice> choices) {
        return repository.saveAll(choices);
    }

    // Fetch by rootId
    public List<GdsMenuChoice> getChoicesByRootId(Long rootId) {
        return repository.findByRootId(rootId);
    }

    // Delete a specific record by rootId and id
    @Transactional
    public void deleteChoice(Long rootId, Long id) {
        repository.deleteByRootIdAndId(rootId, id);
    }

    // Delete all records by rootId
    @Transactional
    public void deleteChoicesByRootId(Long rootId) {
        repository.deleteByRootId(rootId);
    }
}


import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/gdsmenuchoice")
public class GdsMenuChoiceController {

    private final GdsMenuChoiceService service;

    public GdsMenuChoiceController(GdsMenuChoiceService service) {
        this.service = service;
    }

    // Bulk insert or update records
    @PostMapping("/save")
    public ResponseEntity<List<GdsMenuChoice>> saveOrUpdate(@RequestBody List<GdsMenuChoice> choices) {
        return ResponseEntity.ok(service.saveOrUpdateChoices(choices));
    }

    // Fetch records by rootId
    @GetMapping("/{rootId}")
    public ResponseEntity<List<GdsMenuChoice>> getByRootId(@PathVariable Long rootId) {
        List<GdsMenuChoice> choices = service.getChoicesByRootId(rootId);
        return choices.isEmpty() ? ResponseEntity.notFound().build() : ResponseEntity.ok(choices);
    }

    // Delete a single record by rootId and id
    @DeleteMapping("/{rootId}/{id}")
    public ResponseEntity<Void> deleteByRootIdAndId(@PathVariable Long rootId, @PathVariable Long id) {
        service.deleteChoice(rootId, id);
        return ResponseEntity.noContent().build();
    }

    // Delete all records by rootId
    @DeleteMapping("/{rootId}")
    public ResponseEntity<Void> deleteByRootId(@PathVariable Long rootId) {
        service.deleteChoicesByRootId(rootId);
        return ResponseEntity.noContent().build();
    }
}


DELETE /api/gdsmenuchoice/1/5

DELETE /api/gdsmenuchoice/1

// Sort by gdsmuchorder
        choices.sort(Comparator.comparing(GdsMenuChoice::getGdsmuchorder));

        // Reassign order if gaps exist
        IntStream.range(0, choices.size()).forEach(i -> choices.get(i).setGdsmuchorder(i + 1));


^(?!.*(?i)<a\\s*=\\s*https?://).*

----------------------------------------------------------------

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Table(name = "gdsccminspirelookup")
@Data
public class GdsCcmInspireLookup {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Adjust if necessary
    private Long id; // Add a primary key if not already present

    @Column(nullable = false)
    private Integer menunr;

    @Column(nullable = false)
    private Integer optionnr;

    @Column(nullable = false)
    private String varname;

    @Column(nullable = false)
    private String varvalue;
}


import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface GdsCcmInspireLookupRepository extends JpaRepository<GdsCcmInspireLookup, Long> {

    List<GdsCcmInspireLookup> findByMenunrAndOptionnr(Integer menunr, Integer optionnr);

    void deleteByMenunrAndOptionnr(Integer menunr, Integer optionnr);

    void deleteByMenunrInAndOptionnrIn(List<Integer> menunr, List<Integer> optionnr);
}

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
public class GdsCcmInspireLookupService {

    private final GdsCcmInspireLookupRepository repository;

    public GdsCcmInspireLookupService(GdsCcmInspireLookupRepository repository) {
        this.repository = repository;
    }

    public GdsCcmInspireLookup saveOrUpdate(GdsCcmInspireLookup entity) {
        return repository.save(entity);
    }

    public List<GdsCcmInspireLookup> bulkSaveOrUpdate(List<GdsCcmInspireLookup> entities) {
        return repository.saveAll(entities);
    }

    @Transactional
    public void deleteByMenunrAndOptionnr(Integer menunr, Integer optionnr) {
        repository.deleteByMenunrAndOptionnr(menunr, optionnr);
    }

    @Transactional
    public void deleteByMenunrAndOptionnrList(List<Integer> menunrList, List<Integer> optionnrList) {
        repository.deleteByMenunrInAndOptionnrIn(menunrList, optionnrList);
    }
}


import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/inspirelookup")
public class GdsCcmInspireLookupController {

    private final GdsCcmInspireLookupService service;

    public GdsCcmInspireLookupController(GdsCcmInspireLookupService service) {
        this.service = service;
    }

    @PostMapping("/save")
    public ResponseEntity<GdsCcmInspireLookup> saveOrUpdate(@RequestBody GdsCcmInspireLookup entity) {
        return ResponseEntity.ok(service.saveOrUpdate(entity));
    }

    @PostMapping("/saveAll")
    public ResponseEntity<List<GdsCcmInspireLookup>> bulkSaveOrUpdate(@RequestBody List<GdsCcmInspireLookup> entities) {
        return ResponseEntity.ok(service.bulkSaveOrUpdate(entities));
    }

    @DeleteMapping("/delete")
    public ResponseEntity<Void> deleteByMenunrAndOptionnr(@RequestParam Integer menunr, @RequestParam Integer optionnr) {
        service.deleteByMenunrAndOptionnr(menunr, optionnr);
        return ResponseEntity.noContent().build();
    }

    @DeleteMapping("/deleteBulk")
    public ResponseEntity<Void> deleteByMenunrAndOptionnrList(@RequestBody List<DeleteRequest> deleteRequests) {
        List<Integer> menunrList = deleteRequests.stream().map(DeleteRequest::getMenunr).toList();
        List<Integer> optionnrList = deleteRequests.stream().map(DeleteRequest::getOptionnr).toList();
        service.deleteByMenunrAndOptionnrList(menunrList, optionnrList);
        return ResponseEntity.noContent().build();
    }
}

class DeleteRequest {
    private Integer menunr;
    private Integer optionnr;
    
    public Integer getMenunr() {
        return menunr;
    }
    
    public Integer getOptionnr() {
        return optionnr;
    }
}


POST /api/inspirelookup/save
Content-Type: application/json

{
    "menunr": 1,
    "optionnr": 2,
    "varname": "example",
    "varvalue": "value"
}


POST /api/inspirelookup/saveAll
Content-Type: application/json

[
    {
        "menunr": 1,
        "optionnr": 2,
        "varname": "example1",
        "varvalue": "value1"
    },
    {
        "menunr": 3,
        "optionnr": 4,
        "varname": "example2",
        "varvalue": "value2"
    }
]


DELETE /api/inspirelookup/delete?menunr=1&optionnr=2


DELETE /api/inspirelookup/deleteBulk
Content-Type: application/json

[
    { "menunr": 1, "optionnr": 2 },
    { "menunr": 3, "optionnr": 4 }
]


import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class GdsCcmInspireLookupService {

    private final GdsCcmInspireLookupRepository repository;

    public GdsCcmInspireLookupService(GdsCcmInspireLookupRepository repository) {
        this.repository = repository;
    }

    public List<GdsCcmInspireLookup> getAllRecords() {
        return repository.findAll();
    }
}


import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/inspirelookup")
public class GdsCcmInspireLookupController {

    private final GdsCcmInspireLookupService service;

    public GdsCcmInspireLookupController(GdsCcmInspireLookupService service) {
        this.service = service;
    }

    @GetMapping("/all")
    public ResponseEntity<List<GdsCcmInspireLookup>> getAllRecords() {
        return ResponseEntity.ok(service.getAllRecords());
    }
}

GET /api/inspirelookup/all
-------------------------------------------------------------

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Table(name = "gdstext")
@Data
public class GdsText {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Adjust if necessary
    private Long id; // Primary key (If not present in DB, adjust accordingly)

    @Column(nullable = false)
    private Integer rootid;

    @Column(nullable = false)
    private String gdstextname;

    @Column(nullable = false)
    private String gdstextlanguage;

    @Column(nullable = false)
    private String gdstextvalue;
}


import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface GdsTextRepository extends JpaRepository<GdsText, Long> {

    List<GdsText> findByRootid(Integer rootid);

    void deleteByRootid(Integer rootid);
}


import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
public class GdsTextService {

    private final GdsTextRepository repository;

    public GdsTextService(GdsTextRepository repository) {
        this.repository = repository;
    }

    public GdsText saveOrUpdate(GdsText entity) {
        return repository.save(entity);
    }

    public List<GdsText> bulkSaveOrUpdate(List<GdsText> entities) {
        return repository.saveAll(entities);
    }

    public List<GdsText> getByRootid(Integer rootid) {
        return repository.findByRootid(rootid);
    }

    @Transactional
    public void deleteByRootid(Integer rootid) {
        repository.deleteByRootid(rootid);
    }

    @Transactional
    public void deleteByRootidList(List<Integer> rootidList) {
        for (Integer rootid : rootidList) {
            repository.deleteByRootid(rootid);
        }
    }
}


import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/gdstext")
public class GdsTextController {

    private final GdsTextService service;

    public GdsTextController(GdsTextService service) {
        this.service = service;
    }

    @PostMapping("/save")
    public ResponseEntity<GdsText> saveOrUpdate(@RequestBody GdsText entity) {
        return ResponseEntity.ok(service.saveOrUpdate(entity));
    }

    @PostMapping("/saveAll")
    public ResponseEntity<List<GdsText>> bulkSaveOrUpdate(@RequestBody List<GdsText> entities) {
        return ResponseEntity.ok(service.bulkSaveOrUpdate(entities));
    }

    @GetMapping("/get")
    public ResponseEntity<List<GdsText>> getByRootid(@RequestParam Integer rootid) {
        return ResponseEntity.ok(service.getByRootid(rootid));
    }

    @DeleteMapping("/delete")
    public ResponseEntity<Void> deleteByRootid(@RequestParam Integer rootid) {
        service.deleteByRootid(rootid);
        return ResponseEntity.noContent().build();
    }

    @DeleteMapping("/deleteBulk")
    public ResponseEntity<Void> deleteByRootidList(@RequestBody List<Integer> rootidList) {
        service.deleteByRootidList(rootidList);
        return ResponseEntity.noContent().build();
    }
}


POST /api/gdstext/save
Content-Type: application/json

{
    "rootid": 100,
    "gdstextname": "Sample Text",
    "gdstextlanguage": "EN",
    "gdstextvalue": "This is a sample value"
}


POST /api/gdstext/saveAll
Content-Type: application/json

[
    {
        "rootid": 101,
        "gdstextname": "Text A",
        "gdstextlanguage": "EN",
        "gdstextvalue": "Value A"
    },
    {
        "rootid": 102,
        "gdstextname": "Text B",
        "gdstextlanguage": "FR",
        "gdstextvalue": "Valeur B"
    }
]


GET /api/gdstext/get?rootid=100

DELETE /api/gdstext/delete?rootid=100

DELETE /api/gdstext/deleteBulk
Content-Type: application/json

[100, 101, 102]


----------------------------------------------

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Table(name = "gdsccmparatextlookup")
@Data
public class GdsCcmParaTextLookup {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Adjust if necessary
    private Long id; // Add a primary key if not already present

    @Column(nullable = false)
    private Integer paraid;

    @Column(nullable = false)
    private String paratexten;

    @Column(nullable = false)
    private String paratextfr;

    @Column(nullable = false)
    private String paratextdu;

    @Column(nullable = false)
    private String paratextge;
}


import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface GdsCcmParaTextLookupRepository extends JpaRepository<GdsCcmParaTextLookup, Long> {

    List<GdsCcmParaTextLookup> findByParaid(Integer paraid);

    void deleteByParaid(Integer paraid);
}


import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
public class GdsCcmParaTextLookupService {

    private final GdsCcmParaTextLookupRepository repository;

    public GdsCcmParaTextLookupService(GdsCcmParaTextLookupRepository repository) {
        this.repository = repository;
    }

    public GdsCcmParaTextLookup saveOrUpdate(GdsCcmParaTextLookup entity) {
        return repository.save(entity);
    }

    public List<GdsCcmParaTextLookup> bulkSaveOrUpdate(List<GdsCcmParaTextLookup> entities) {
        return repository.saveAll(entities);
    }

    public List<GdsCcmParaTextLookup> getByParaid(Integer paraid) {
        return repository.findByParaid(paraid);
    }

    @Transactional
    public void deleteByParaid(Integer paraid) {
        repository.deleteByParaid(paraid);
    }

    @Transactional
    public void deleteByParaidList(List<Integer> paraidList) {
        for (Integer paraid : paraidList) {
            repository.deleteByParaid(paraid);
        }
    }
}


import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/paratextlookup")
public class GdsCcmParaTextLookupController {

    private final GdsCcmParaTextLookupService service;

    public GdsCcmParaTextLookupController(GdsCcmParaTextLookupService service) {
        this.service = service;
    }

    @PostMapping("/save")
    public ResponseEntity<GdsCcmParaTextLookup> saveOrUpdate(@RequestBody GdsCcmParaTextLookup entity) {
        return ResponseEntity.ok(service.saveOrUpdate(entity));
    }

    @PostMapping("/saveAll")
    public ResponseEntity<List<GdsCcmParaTextLookup>> bulkSaveOrUpdate(@RequestBody List<GdsCcmParaTextLookup> entities) {
        return ResponseEntity.ok(service.bulkSaveOrUpdate(entities));
    }

    @GetMapping("/get")
    public ResponseEntity<List<GdsCcmParaTextLookup>> getByParaid(@RequestParam Integer paraid) {
        return ResponseEntity.ok(service.getByParaid(paraid));
    }

    @DeleteMapping("/delete")
    public ResponseEntity<Void> deleteByParaid(@RequestParam Integer paraid) {
        service.deleteByParaid(paraid);
        return ResponseEntity.noContent().build();
    }

    @DeleteMapping("/deleteBulk")
    public ResponseEntity<Void> deleteByParaidList(@RequestBody List<Integer> paraidList) {
        service.deleteByParaidList(paraidList);
        return ResponseEntity.noContent().build();
    }
}


POST /api/paratextlookup/save
Content-Type: application/json

{
    "paraid": 1,
    "paratexten": "Sample Text EN",
    "paratextfr": "Exemple Texte FR",
    "paratextdu": "Voorbeeldtekst DU",
    "paratextge": "Beispieltext GE"
}


POST /api/paratextlookup/saveAll
Content-Type: application/json

[
    {
        "paraid": 2,
        "paratexten": "Text A EN",
        "paratextfr": "Texte A FR",
        "paratextdu": "Tekst A DU",
        "paratextge": "Text A GE"
    },
    {
        "paraid": 3,
        "paratexten": "Text B EN",
        "paratextfr": "Texte B FR",
        "paratextdu": "Tekst B DU",
        "paratextge": "Text B GE"
    }
]


GET /api/paratextlookup/get?paraid=1


DELETE /api/paratextlookup/delete?paraid=1


DELETE /api/paratextlookup/deleteBulk
Content-Type: application/json

[1, 2, 3]


