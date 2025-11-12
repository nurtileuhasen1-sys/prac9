# prac9

class AccommodationService {
    void registerGuest(String clientName) {
        System.out.println("Guest registration completed for: " + clientName);
    }
    void vacateRoom(String clientName) {
        System.out.println("Accommodation cancelled for: " + clientName);
    }
}

class CulinaryOperations {
    void bookDiningTable(String clientName) {
        System.out.println("Table reservation confirmed for: " + clientName);
    }
    void processMealOrder(String clientName) {
        System.out.println("Meal preparation started for guest: " + clientName);
    }
}

class ConferenceLogistics {
    void secureVenue(String eventLabel) {
        System.out.println("Venue secured for conference: " + eventLabel);
    }
    void procureAVEquipment(String eventLabel) {
        System.out.println("A/V equipment ordered for: " + eventLabel);
    }
}

class JanitorialUnit {
    void deployCleaningTeam(String locationId) {
        System.out.println("Standard cleaning scheduled for: " + locationId);
    }
    void urgentCleanup(String locationId) {
        System.out.println("Immediate deep cleaning dispatched to: " + locationId);
    }
}

class TransportationDesk {
    void arrangeVehicle(String clientName) {
        System.out.println("Private car transfer arranged for: " + clientName);
    }
}


class HospitalityManager {
    private AccommodationService accommodation;
    private CulinaryOperations culinary;
    private ConferenceLogistics conference;
    private JanitorialUnit janitorial;
    private TransportationDesk transport;

    public HospitalityManager(AccommodationService a, CulinaryOperations c, ConferenceLogistics cl, JanitorialUnit j, TransportationDesk t) {
        this.accommodation = a;
        this.culinary = c;
        this.conference = cl;
        this.janitorial = j;
        this.transport = t;
    }

    void fullServiceCheckIn(String guest) {
        System.out.println("--- Starting Full Service Check-In for " + guest + " ---");
        accommodation.registerGuest(guest);
        culinary.processMealOrder(guest);
        janitorial.deployCleaningTeam("Guest Room for " + guest);
        System.out.println("--- Check-In Complete ---");
    }

    void coordinateMajorEvent(String eventTitle) {
        System.out.println("--- Coordinating Major Event: " + eventTitle + " ---");
        conference.secureVenue(eventTitle);
        conference.procureAVEquipment(eventTitle);
        accommodation.registerGuest("Event Staff for " + eventTitle);
        System.out.println("--- Event Setup Complete ---");
    }

    void quickDiningAndTransfer(String guestName) {
        culinary.bookDiningTable(guestName);
        transport.arrangeVehicle(guestName);
    }
    
    void requestUrgentCleaning(String roomNumber) {
        janitorial.urgentCleanup(roomNumber);
    }
}

public class ManagerFacadeDemo {
    public static void main(String[] args) {
        AccommodationService acc = new AccommodationService();
        CulinaryOperations cul = new CulinaryOperations();
        ConferenceLogistics conf = new ConferenceLogistics();
        JanitorialUnit jan = new JanitorialUnit();
        TransportationDesk trans = new TransportationDesk();
        
        HospitalityManager manager = new HospitalityManager(acc, cul, conf, jan, trans);
        
       
        manager.fullServiceCheckIn("Hasen");
        System.out.println();
        manager.coordinateMajorEvent("Investor Summit");
        System.out.println();
        manager.quickDiningAndTransfer("Nurtiley");
        System.out.println();
        manager.requestUrgentCleaning("Room 501");
    }
}
import java.util.ArrayList;
import java.util.List;

abstract class CorporateUnit {
    protected String label;
    public CorporateUnit(String label) {
        this.label = label;
    }
   
    public abstract void printHierarchy(int indentLevel); 
   
    public abstract double calculatePayrollCost();
   
    public abstract int countActivePersonnel();
    
  
    public void addChild(CorporateUnit unit) {
        throw new UnsupportedOperationException();
    }
    public void removeChild(CorporateUnit unit) {
        throw new UnsupportedOperationException();
    }
    public CorporateUnit locateUnit(String unitName) {
        return null;
    }
}

class Personnel extends CorporateUnit {
    private String jobTitle;
    private double yearlySalary;
    
    public Personnel(String name, String title, double salary) {
        super(name);
        this.jobTitle = title;
        this.yearlySalary = salary;
    }
    
    public double calculatePayrollCost() {
        return yearlySalary;
    }
    
    public int countActivePersonnel() {
        return 1;
    }
    
    public void printHierarchy(int indentLevel) {
        System.out.println(" ".repeat(indentLevel) + "-> Staff: " + label + " (" + jobTitle + ", Salary: $" + yearlySalary + ")");
    }
    
    public CorporateUnit locateUnit(String unitName) {
        return this.label.equalsIgnoreCase(unitName) ? this : null;
    }
}

class ExternalPartner extends CorporateUnit {
    private double retainerFee;
    
    public ExternalPartner(String name, double fee) {
        super(name);
        this.retainerFee = fee;
    }
    
    public double calculatePayrollCost() {
        return 0; // Не является штатным сотрудником
    }
    
    public int countActivePersonnel() {
        return 0;
    }
    
    public void printHierarchy(int indentLevel) {
        System.out.println(" ".repeat(indentLevel) + "-> Contractor: " + label + " (Fee: $" + retainerFee + ")");
    }
    
    public CorporateUnit locateUnit(String unitName) {
        return this.label.equalsIgnoreCase(unitName) ? this : null;
    }
}

class OperationalDepartment extends CorporateUnit {
    private List<CorporateUnit> componentList = new ArrayList<>();
    
    public OperationalDepartment(String label) {
        super(label);
    }
    
    public void addChild(CorporateUnit unit) {
        componentList.add(unit);
    }
    
    public void removeChild(CorporateUnit unit) {
        componentList.remove(unit);
    }
    
    public double calculatePayrollCost() {
        double totalCost = 0;
        for (CorporateUnit unit : componentList) totalCost += unit.calculatePayrollCost();
        return totalCost;
    }
    
    public int countActivePersonnel() {
        int totalCount = 0;
        for (CorporateUnit unit : componentList) totalCount += unit.countActivePersonnel();
        return totalCount;
    }
    
    public void printHierarchy(int indentLevel) {
        System.out.println(" ".repeat(indentLevel) + "** Department: " + label + " **");
        for (CorporateUnit unit : componentList) unit.printHierarchy(indentLevel + 4);
    }
    
    public CorporateUnit locateUnit(String unitName) {
        if (this.label.equalsIgnoreCase(unitName)) return this;
        for (CorporateUnit unit : componentList) {
            CorporateUnit found = unit.locateUnit(unitName);
            if (found != null) return found;
        }
        return null;
    }
}

public class CorporateStructureDemo {
    public static void main(String[] args) {
        OperationalDepartment mainHQ = new OperationalDepartment("Central Headquarters");
        OperationalDepartment technologyDiv = new OperationalDepartment("Technology Division");
        OperationalDepartment talentAcquisition = new OperationalDepartment("Talent Acquisition");
        
        Personnel p1 = new Personnel("Aigerim", "Executive Director", 150000);
        Personnel p2 = new Personnel("Daniyar", "Senior Engineer", 120000);
        Personnel p3 = new Personnel("Askar", "Recruitment Lead", 90000);
        ExternalPartner ext1 = new ExternalPartner("Murat Consulting", 80000);
        
        technologyDiv.addChild(p2);
        technologyDiv.addChild(ext1);
        talentAcquisition.addChild(p3);
        
        mainHQ.addChild(p1);
        mainHQ.addChild(technologyDiv);
        mainHQ.addChild(talentAcquisition);
        
        mainHQ.printHierarchy(0);
        
        System.out.println("\nTotal Annual Payroll Cost: $" + mainHQ.calculatePayrollCost());
        System.out.println("Total Full-Time Staff: " + mainHQ.countActivePersonnel());
        
        CorporateUnit located = mainHQ.locateUnit("Daniyar");
        if (located != null) {
            System.out.println("\nUnit Located: " + located.label + " (Type: " + located.getClass().getSimpleName() + ")");
        }
    }
}
